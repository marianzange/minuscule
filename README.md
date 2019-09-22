# Minuscule

Miniscule is a tiny TCP command server that can be used to trigger local commands on the host it's running on. It uses netcat to listen to a TCP socket and coprocesses for the read and write streams.

The code contains termination of a DCOS slave node as an example since I have used this approach for cluster node lifecycle management. You can simply replace this with whatever you need though.

## Starting Minuscule

To start Minuscule, just execute it with the port you want to listen on.

```bash
$ minuscule 5039
```

## Protocol

### Message Format

When exchanging messages with Minuscule, the server expects the following messaging format. All messages must be terminated with a new-line `\n`.

#### Request

```
<COMMAND><TERMINATION>
```

#### Response

The first segment specifies if the request was succesful. It's `0` if it was successful and `1` if not.

```
<0|1>:<ERROR_CODE|MESSAGE><TERMINATION>
```

## Available Commands

```
TERMINATE  Sends SIGUSR1 to the Mesos slave process and tells systemctl to stop the service
```

## Error Codes

```
100  The command is invalid
200  A sub-command execution failed with a non-zero exit code

```
