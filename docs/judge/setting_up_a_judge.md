# Setting up a judge

This guide explains how to install a judge and connect it to the site.  
It is intended for Linux-based machines (WSL included). Windows is not supported.

It assumes you have already installed the site and that a bridge instance is running.

---

## Site-side setup

Go to **/admin/judge/** in the Django admin panel and create a new judge.  
Give it a name and generate an authentication key using the **Regenerate** button.

In `local_settings.py`, locate:

```python
BRIDGED_JUDGE_ADDRESS = "localhost:9999"
````

This is the address judges will connect to.
If your judge is on a different machine, change `localhost` to your server’s IP and ensure the port is **open**.

Confirm the bridge is running:

```shell-session
$ supervisorctl status
bridged RUNNING pid <pid>, uptime <uptime>
```

---

## Judge-side setup

Judges can be installed via Docker or via the PyPI package.
Docker is strongly recommended as it bundles all required runtimes.

### With Docker

#### Pre-built images

We rely on VNOJ’s Docker runtime images:

**K23OJ uses `vnoj/judge-tiervnoj`, which includes Tier 1 runtimes and additional languages used in Vietnamese contests.**

Available images:

* `vnoj/judge-tiervnoj`
* `vnoj/judge-tier1`
* `vnoj/judge-tier2`
* `vnoj/judge-tier3` *(legacy / not recommended)*

Supported runtimes:
[https://k23oj.io.vn/runtimes](https://k23oj.io.vn/runtimes)

---

#### From source

To build the `judge-tiervnoj` image yourself:

```shell-session
$ git clone --recursive https://github.com/VNOI-Admin/judge-server.git
$ cd judge/.docker
$ make judge-tiervnoj
```

---

### Running a judge container

This example runs a `tiervnoj` judge on the same server as the site.
It expects:

* Problems located at `/mnt/problems`
* Judge configuration at `/mnt/problems/judge.yml`

For new deployments:
Both the site and judge may share the same folder as long as
`DMOJ_PROBLEM_DATA_ROOT` points to the same directory on the site.

Example `judge.yml`:

```yaml
id: <judge name>
key: <judge authentication key>
problem_storage_globs:
  - /problems/*
```

Run the judge:

```shell-session
$ docker run \
    --name judge \
    --network="host" \
    -v /mnt/problems:/problems \
    --cap-add=SYS_PTRACE \
    -d \
    --restart=always \
    vnoj/judge-tiervnoj:latest \
    run -p 9999 -A 0.0.0.0 -a 12345 -c /problems/judge.yml localhost
```

If you changed `BRIDGED_JUDGE_ADDRESS` in the site config, update `-p 9999` accordingly.

---

### Running multiple judges

If you want multiple judge instances, modify:

* `--name judge` → different container name
* `/problems/judge.yml` → different config file
* `-a 12345` → different judge-exposed port

All judges may connect to the same bridge port (`-p 9999`) unless using multiple bridges.