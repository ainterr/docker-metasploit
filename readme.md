# docker-metasploit

A dockerfile for building a metasploit container - useful when you don't want
to install all of metasploit's dependencies or need to get up and running
quickly. Follows the instructions
[here](https://www.darkoperator.com/installing-metasploit-in-ubunt/).

## Building

```bash
docker build metasploit -t metasploit
```

## Running

1. Start the container

    ```bash
    docker run -d --name metasploit -p 443:443/tcp metasploit
    ```

    You can add additional port ranges or shared volumes if necessisary - see [docker's run documentation](https://docs.docker.com/engine/reference/run/).

2. Exec into the container

    ```bash
    docker exec -it --user root metasploit bash
    ```
