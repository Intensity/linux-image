# linux-image: a repository for reproducible Linux containers focused on security and stability

## Images

- `devuan-4-hardenedmalloc`

## Build process

With `podman` installed, the `devuan-4-hardenedmalloc` image can be created with

```bash
    $ ./Environment/devuan-4-hardenedmalloc/Containerfile
```

## Interactive use

The produced image can also be tested interactively. An example
instantiation for `podman` is below.  Note that arguments that are
restrictive (such as no networking, read-only mode, or restrictive
capabilities and permissions) can be removed if necessary. The example
includes a means to inject host directories into the container. Another
option is to run processes in the container as a non-root user.

```bash
    $ /usr/bin/podman --runtime crun --storage-driver vfs --cgroup-manager=cgroupfs --events-backend file run --interactive --tty --rm --pull never --rm --quiet --name devuan-4-hardenedmalloc --cpus 1 --label=name=devuan-4-hardenedmalloc --hostname devuan-4-hardenedmalloc --no-hosts --cap-drop ALL --security-opt no-new-privileges --mount type=bind,source=/opt,target=/opt/host --tz Universal --env TZ=Universal --env TERM=vt220 --volume /opt:/opt/host2 --detach-keys 'ctrl-^,ctrl-^' --read-only --network none -- intensity/linux-image:devuan-4-hardenedmalloc
```

## Emphasis for `devuan-4-hardenedmalloc` image:

- Leverage stability of Debian
- Focus on a foundation without systemd (Devuan), in an effort to reduce complexity and increase modularity
- Include foundational packages focused on security, forensics, networking, compression, basic/minimal build toolchains
- Use hardened memory allocator by default for dynamic executables (from Kicksecure intermediate image)
- Remove setuid and setgid bit for relevant binaries by default, since many use cases do not call for these. Though the permissions can be restored dynamically by the `root` user
- Prefer static binaries for critical processes such as shells
- Choose a minimal dependencies by not installing recommended and suggested packages by default, and not bringing in packages with heavy dependencies
- Let PATH point to standard system binaries by default, instead of preferring local binaries first
- Choose restrictive umask
- Suggest some aliases for secure editing
- Remove data from logs so as to reveal less about the environment that built the container
- Include packages to facilitate security, networking, compression, and system administration
- Deliberately exclude some packages/binaries (such as OpenSSH) to make room for a custom minimal and secure build (though TinySSH and Drobear are available)

## Additional Kicksecure intermediate image:
- Leverage Kicksecure project for its secure foundations, including for example the hardened allocator. Though not all measures are applicable in a container, the overall aim is to produce an image, which may be used in a container or alternatively for other purposes
