# Pi-Hole

## Updating the blocklist via shell

First, you need to open a bash shell to the pihole container.

```Shell
docker exec -it pihole /bin/bash
```

Then you can update the blocklist.

```Shell
pihole -g
```

## References

- [Pi-hole.net](https://pi-hole.net/)
