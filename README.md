# `diff_ids` exercise

```sh
podman build -f Containerfile -t quay.io/mmortari/demo20241129-diffids .
podman push quay.io/mmortari/demo20241129-diffids
skopeo copy docker://quay.io/mmortari/demo20241129-diffids oci:downloaded:latest
cp downloaded/blobs/sha256/95c8d90961239c600de6c31a184e45f18fba83855e10c788c8451cea210416a4 downloaded/layer0.tar.gz
gunzip -k downloaded/layer0.tar.gz 
tar tf downloaded/layer0.tar
```

The obvious:

```
cat downloaded/layer0.tar.gz | shasum -a 256 | awk '{print $1}'
95c8d90961239c600de6c31a184e45f18fba83855e10c788c8451cea210416a4

ls -la downloaded/blobs/sha256/95c8d90961239c600de6c31a184e45f18fba83855e10c788c8451cea210416a4
-rw-r--r--@ 1 mmortari  staff  155 Nov 29 17:35 downloaded/blobs/sha256/95c8d90961239c600de6c31a184e45f18fba83855e10c788c8451cea210416a4
```

The less obvious:

```
cat downloaded/layer0.tar | shasum -a 256 | awk '{print $1}' 
22912a19b55d28408ca62535c330b57a68456fcc8646e5b6ef4132000d97d023

cat downloaded/blobs/sha256/a46765fa418570a2ecd6fc40301ed7e320c9a1973ecc166954d0c18323aa1a25 | jq | grep -B 3 -A 3 22912a19b55d28408ca62535c330b57a68456fcc8646e5b6ef4132000d97d023
  "rootfs": {
    "type": "layers",
    "diff_ids": [
      "sha256:22912a19b55d28408ca62535c330b57a68456fcc8646e5b6ef4132000d97d023"
    ]
  },
  "history": [
```
