Travis CI test matrix
=====================

GCE instances
-------------

Here is the test matrix for the Travis CI gates:

|           Network plugin|                  OS type|               GCE region|             Nodes layout|
|-------------------------|-------------------------|-------------------------|-------------------------|
|                    canal|       debian-8-kubespray|             asia-east1-a|                       ha|
|                   calico|       debian-8-kubespray|           europe-west1-c|                  default|
|                  flannel|                 centos-7|        asia-northeast1-c|                  default|
|                   calico|                 centos-7|            us-central1-b|                       ha|
|                    weave|                   rhel-7|               us-east1-c|                  default|
|                    canal|            coreos-stable|               us-west1-b|                  default|
|                    canal|                   rhel-7|        asia-northeast1-b|                 separate|
|                    weave|       ubuntu-1604-xenial|           europe-west1-d|                 separate|
|                   calico|            coreos-stable|            us-central1-f|                 separate|

Where the nodes layout `default` is a non-HA two nodes setup with the separate `kube-node`
and the `etcd` group merged with the `kube-master`. The `separate` layout is when
there is only node of each type, which is a kube master, compute and etcd cluster member.
And the `ha` layout stands for a two etcd nodes, two masters and a single worker node,
partially intersecting though.

Note, the canal network plugin deploys flannel as well plus calico policy controller.

Hint: the command
```
bash scripts/gen_matrix.sh
```
will (hopefully) generate the CI test cases from the current ``.travis.yml``.

Gitlab CI test matrix
=====================

GCE instances
-------------

|               Stage|      Network plugin|             OS type|          GCE region|        Nodes layout
|--------------------|--------------------|--------------------|--------------------|--------------------|
|               part1|              calico|       coreos-stable|          us-west1-b|           separated|
|               part1|               canal|  debian-8-kubespray|          us-east1-b|                  ha|
|               part1|               weave|              rhel-7|      europe-west1-b|             default|
|               part2|             flannel|            centos-7|          us-west1-a|             default|
|               part2|              calico|  debian-8-kubespray|       us-central1-b|             default|
|               part2|               canal|       coreos-stable|          us-east1-b|             default|
|             special|               canal|              rhel-7|          us-east1-b|           separated|
|             special|               weave|  ubuntu-1604-xenial|       us-central1-b|           separated|
|             special|              calico|            centos-7|      europe-west1-b|                  ha|
|             special|               weave|        coreos-alpha|          us-west1-a|                  ha|

The "Stage" means a build step of the build pipeline. The steps are ordered as `part1->part2->special`.
