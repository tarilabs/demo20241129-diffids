# `diff_ids` exercise

```sh
podman build -f Containerfile -t quay.io/mmortari/demo20241129-diffids .
podman push quay.io/mmortari/demo20241129-diffids
skopeo copy docker://quay.io/mmortari/demo20241129-diffids oci:downloaded:latest
cp downloaded/blobs/sha256/b413ca4e6e8623325374d4552dbe5a39ebef0576c14571d09dfb7c9a49f0fbeb downloaded/layer0.tar.gz
gunzip -k downloaded/layer0.tar.gz 
```

Showing the results in layer0:

```
tar tf downloaded/layer0.tar
models/
models/hello.md
models/world.md
```

The obvious:

```
cat downloaded/layer0.tar.gz | shasum -a 256 | awk '{print $1}'
b413ca4e6e8623325374d4552dbe5a39ebef0576c14571d09dfb7c9a49f0fbeb

ls -la downloaded/blobs/sha256/b413ca4e6e8623325374d4552dbe5a39ebef0576c14571d09dfb7c9a49f0fbeb
-rw-r--r--@ 1 mmortari  staff  196 Nov 29 17:47 downloaded/blobs/sha256/b413ca4e6e8623325374d4552dbe5a39ebef0576c14571d09dfb7c9a49f0fbeb
```

The less obvious:

```
cat downloaded/layer0.tar | shasum -a 256 | awk '{print $1}' 
a559cc7062e46d6a7c776ce402549a62523de4898116ba480754a5bc808f0ba9

cat downloaded/blobs/sha256/ec75ef8b2d0e40d01c706bcc2b232950795867f9d20da00fb3317ff2bc73202c | jq | grep -B 3 -A 3 a559cc7062e46d6a7c776ce402549a62523de4898116ba480754a5bc808f0ba9

  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:a559cc7062e46d6a7c776ce402549a62523de4898116ba480754a5bc808f0ba9"
    ]
  },
  "history": [
```
