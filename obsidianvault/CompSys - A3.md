*Asger Skov Velling - nbz132*

## Theoretical part
### 1.2 HTTP
##### 1.2.1 HTTP semantics
> *HTTP employs a message format divided into header and body sections. The initial header of a request consists of a method field, a resource identifier field and a protocol version field. Likewise, the initial header of a response consists of a protocol version field, a status code and an optional message.*
> **Part 1:** What is the purpose of the method field in requests? How do POST and GET requests differ in practice?

The various HTTP verbs tell the server what kind of action to take.
A GET request means the client wants to get a resource from the server.
A POST request means the client wants to create a resource on the server.

> **Part 2:** An additional (and mandatory) header field is the Host header. Why is this header necessary?

The host header allows the client to specify a destination IP address and port. That is useful because one server often hosts more than one HTTP server, which is achieved with virtual hosting.

##### 1.2.2 HTTP headers and fingerprinting
> **Part 1:** One of the additional header fields is Set-cookie (for responses) and cookie. What is the reason for these header fields and to what degree may they be used as a unique identifier?

Cookies provide the server with a way to store small amounts of data on the client.
They are often used as a way to authenticate a user, trying to make a POST request to /login. The server sets a unique session ID as a cookie in its response. The following requests made by the client will contain the session ID, identifying them as a specific user.
It must be pointed out, that the user's session ID does not guarantee their authenticity, since it can be compromised by a replay attack.

> Explain the advantages of the CNAME type records. Explain how DNS may provide simple load balancing among servers. (Answer with 2-4 sentences.)

Since a canonical name record creates an alias for a domain name, it can be used to direct traffic. If we change the domain name of our website, we can simply create a CNAME record that directs users from old-domain.com to new-domain.com.
A naive load balancer an be created using a technique called round robin DNS. If we have multiple app servers, we can keep changing (rotating) the IP address it points to, such as `appserver1.example.com`, `appserver2.example.com`, etc.

> Many DNS servers, especially root and top level domain (TLD) servers, respond with ‘iterative’ replies to recursive requests. Explain the differences between iterative and recursive lookups and when and why recursive lookups are justified. (Answer with 4-8 sentences)

In a recursive lookup, the DNS server takes full responsibility for the name resolution. Often, a DNS client will ask a recursive resolver for a name resolution. The recursive resolver can then start the resolution process, which may be iterative.
In an iterative lookup, a DNS server responds with the best information it knows. It does not guarantee a name resolution. An iterative reply from a root server will include the correct TLD server for the given TLD. The recursive resolver can now ask the correct TLD server for the authoritative name server that knows the IP address of the domain.
Recursive lookups are justified when we want to guarantee a name resolution, and when we want to reduce the workload of the client.


## Programming part
I have created a client for a file sharing server, using the protocol in the assignment text. 
Clients can authenticate to the file sharing server, by sending a salted hash of their username.
Users can then ask for files until they quit the program, by sending a signed request for a file stored on the server.

### Module structure
I have changed the structure of the application. `networking.c` is still called `networking.c`, but I have added a lot of other header and implementation files. Furthermore, I have removed `networking.h` and moved its contents down to a lower abstraction level - inside `hashing.h`.

### The main function
When a user runs the client program, they are first prompted to set a username and a password. This part was not made by me, but handed out.
If the user has not registered with the client before, a new salt is created for them, which will be used in future sessions.
After receiving the salt, the user can request a file by typing its filename into the terminal.
If everything went well, the user now has a copy of the file on their local machine. If not, the response is logged to a file for troubleshooting.
The user can keep requesting files or type `quit` to quit the program.

### Salting
When a user registers, they are either a new user or a returning user.
If they are a new user, a salt is generated for them and placed in a file. The client has a directory called `SALTS` that stores up to one salt for every unique username that has used that client. 
If they are a returning user, they get the salt stored in their designated salt file.

The filename of the salt is the SHA256 hash of their username. I would have liked that filename to be a hash of their username and a secret key. The secret key would act as a salt, and would be stored as an environment variable - not in version control. This would help protect from an adversary trying to get our salt files. If the adversary got a hold of our salt files, they wouldn't be able to use it in combination with a rainbow table to get the right signatures and do a replay attack. Time did not allow it.

### Requesting
The main bulk of the communication with the server goes on in `networking.c`:
```C
int send_and_receive(Request_t *request, FILE *fp);
```

It sends a request to the server and, if successful, receives a response from the server.
If it fails to get a response from the server, `-1` is returned.
If it does get a response from the server, a status code from the protocol is returned.
If that status code is `1`, meaning `OK`, the requested file is written to the file specified by `fp`.

`send_and_receive` is used in both of the functions given to us to fill out: `register_user` and `get_file`.

`register_user` takes a username, a password and a salt.
It initializes a `Request_t`, a struct defined in `hashing.h` (should have been defined elsewhere - see shortcomings).
It then calls `send_and_receive` with the request, and `stdout` as its output file, so the user can get some feedback while using the client.

`get_file` does almost the same, except it uses a file pointer that matches the name of the file on the server. The file then gets written to that file.

### Logging
If any status code other than `1 - OK` is returned from the server, `send_and_receive` logs the response object to a file and tells the user where to find it. 
Here is the log file for a bad request:
```
← Response {
	Length: 34
	Status: Bad request
	Block: 0
	Total blocks: 1
	Block hash: 42d0929153ca29c0676adefb36578e6f...
	Total hash: 42d0929153ca29c0676adefb36578e6f...
	Payload: Requested content a does not exist
}
```

### Ambiguities
- The empty functions provided in `networking.c` seem like they are a core part of the assignment, so I did not change their signatures.
- We have all been using either WSL or some kind of native Linux in school, so I assume it is okay to use POSIX functions like `stat` and `mkdir`. I am programming on a Linux machine and would not expect this program to work on Windows. It might work on a Mac.

### Compiling and running
First, run the python server

```
cd python/server/
python3 server.py ../../config.yaml
```

In another terminal, build and run the C client with

```
cd src
make
./networking ../config.yaml
```

Running `make` will compile everything and run the unit tests.
Running `make clean` will remove all executable files, salts and log files.

### String Policy
I found it necessary to come up with a string policy, just to make the code fit in my head. 
What I decided was to use fixed length strings rather than relying on null terminators. For example, a username is between 1 and `USERNAME_LEN` bytes long. To create it safely, I first zero a buffer and then I copy up to `USERNAME_LEN` bytes of the username into the buffer. 

### Testing
I have written some unit tests for this application. None of them use the network. Instead, they make me feel safer about strings and salts.
I have also tested that the struct `Request_t` is correctly created using `requests.c:create_request()` and that it is not susceptible to buffer overflow attacks. You can run `make test` to run the tests.
```
$ make test
[Test passed]   Salts are not identical - RNG is seeded correctly
[Test passed]   Same input yields same hash
[Test passed]   Register header correctly partitioned
[Test passed]   Request fields do not overflow into each other
[Test passed]   Short strings are zero padded
[Test passed]   Returning users get same salt
```

Besides unit testing, I confirmed that my file requests were being processed correctly. I did that by running the client, requesting a file and then running `diff` on it and the original stored on the server.
In both the case of `tiny.txt` and `hamlet.txt`, there was no difference between those files.

### Shortcomings
- `fileio.c:send_and_receive()` ended up being a long and messy function. It has some unnecessary copying of data. That is because I forgot to check the checksums returned by the server, and only implemented it a few hours before the deadline. I made a hacky solution where I copy an array of `Request_t`'s to a string after having already copied them from a string to an array of `Request_t`. 
- Ideally, `Request_t` should be defined in `requests.h`.
- Integration tests would have been nice to have. The only tests I have are ones that do not use sockets.
- If you run the client before running the server, you will correctly see the message `Failed to open connection to server` in the terminal. However, if you then run the server, while the client is running, and try again, no register request will be sent. That is an error.
- I reverted a commit, and now files get saved to local storage - even on error.

### Conclusion
Generally the programs works as expected. At times, it is a little rough around the edges, but I have not experienced any errors apart from the ones discussed in Shortcomings.
The tests are good enough, but ideally I would have liked to have automated tests that involve the server.
The files are sent successfully. The security is not great, but at least we are hashing and salting. The connection is susceptible to replay attacks, as addressed in Salting.