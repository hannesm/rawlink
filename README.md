# Rawlink (WIP)

Rawlink is an ocaml library for sending and receiving raw packets at the link
layer level.  Sometimes you need to have full control of the packet, including
building the a full ethernet frame.

The API is platform independent and it uses BPF on real UNIXes and AF_SOCKET on
linux. Some functionality is sacrificed so that the API is portable
enough.

Currently BPF AF are implemented, including receiving filtering
capabilities. Writing a BPF program is a pain in the ass, so no facilities are
provided for writing a BPF program. If you need a BPF filter, I suggest you
write a small .c file with a function that returns the BPF program as a string,
check `rawlink_stubs.c` for an example.

Both normal blocking functions as well as `Lwt` monadic variants are provided.

A tipical code for receiving all packets and just sending them back on an
especified interface are detailed below:

```
let link = Rawlink.open_link "eth0" in
let buf = Rawlink.read_packet link in
Printf.printf "got a packet with %d bytes.\n%!" (Cstruct.len buf);
Rawlink.send_packet link buf
```

Check the mli interface for more options.
