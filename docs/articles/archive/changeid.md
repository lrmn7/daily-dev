# Change user id and group id of user

**Last Modified**: March 1, 2020


If you run into an issue where you need to change the user id (`uid`) and group id (`gid`) of a user (`<user>`). Assume you want to change `uid:gid` from `1001:1001` (`<old_uid>:<old_gid>`) to `1000:1000` (`<new_uid>:<new_gid>`).

## Steps by step guide

1. Login using a different user. If need `root` user: `ssh root@<hostname>`.

2. Kill all the process of `user`. Find all process belonging to `<user>` using `ps`:

    ```bash
    ps aux | grep <user>
    ```

3. Change `uid` of `<user>`:

    ```bash
    usermod -u <new_uid> <user>
    ```

4. Change `gid` of `user`:

    ```bash
    groupmod -g <new_gid> <user>
    ```

5. Change ownership of all files from `<old_uid>:<old_gid>` to `<new_uid>:<new_gid>`:

    ```bash
    find /parent/path/ \( -uid <old_uid> -o -gid <old_gid> \) \
      -exec chown <new_uid>:<new_gid> {} \;
    ```

    Example:

    ```bash
    find /home/lento \( -uid 1001 -o -gid 1001 \) \
      -exec chown 1000:1000 {} \;
    ```
