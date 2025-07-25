# Ronin Snapshot

## Note
- We capture a comprehensive snapshot of **full node data**, including pruned state data, once per week.

## Prerequisites
- Your free disk space has more than twice the size of the snapshot.
- Install the [zstd](https://github.com/facebook/zstd) on your machine.
- Install the [tmux](https://github.com/tmux/tmux/wiki/Installing) for long time process operation.


### Endpoint

Here are snapshots of HBSS with leveldb and pebbledb.

#### Hash-Base-State-Scheme:

- **Mainnet LevelDB**: [chaindata-20250726.tar.zst](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/chaindata-20250726.tar.zst)
  - MD5: 87d7215ed00ec77860c1e3c2bb068b16
  - Size: 499G
  - Inspect File: [inspect-data-20250726.txt](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/inspect-data-20250726.txt)

- **Mainnet PebbleDB**: [pebbledb-chaindata-20250726.tar.zst](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/pebbledb-chaindata-20250726.tar.zst)
  - MD5: f9ffffa65ebf026f2405844be53043dd
  - Size: 497G





- **Testnet PebbleDB**: [pebbledb-testnet-chaindata-20250518.tar.zst](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/pebbledb-testnet-chaindata-20250518.tar.zst)
  - MD5: 53759440ae043bbf2209679bae8dfe0d
  - Size: 54G



#### Path-Base-State-Scheme:
For PBSS snapshot, we only support PebbleDB engine:

- **Mainnet PBSS**: [pbss-chaindata-20250726.tar.zst](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/pbss-chaindata-20250726.tar.zst)
  - MD5: 1c1ddc88a7225b1aec0e7a92dbe9f744
  - Size: 480G



- **Testnet PBSS**: [pbss-testnet-chaindata-20250726.tar.zst](https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/pbss-testnet-chaindata-20250726.tar.zst)
  - MD5: 9d9def5182f0d281d3b4d258087d48c1
  - Size: 60G


### Usage

Step 1: Preparation
- Make sure your hardware meets the [suggested requirement](https://docs.roninchain.com/validators/setup/overview#hardware-requirements).
- A disk with enough free storage, at least twice the size of the snapshot.

Step 2: Download & Uncompress
- Copy the above snapshot URL.
- Download:  `wget -O chaindata.tar.zst  "<paste snapshot URL here>"` . It will take one or two hours to download the snapshot, you can put it in to the `tmux` by `wget -O chaindata.tar.gz  "<paste snapshot URL here>"`


* [OPTIONAL] If you need to speedup download, just use [aria2c](https://github.com/aria2/aria2)
```
aria2c -o chaindata.tar.zst -s14 -x14 -k100M https://pub-3cca138de6c349f8afe5f6635f9f6f81.r2.dev/data/{filename}
```

But aria2c may fail sometimes, you need to rerun the download command. To make it convient, you can use the following script, save it into file `download.sh`, open new `tmux` session and run: `chmod +x download.sh && ./download.sh "<paste snapshot URL here>" <your dir>`
```
#!/bin/bash
if [ $# -eq 1 ]; then
        dir=$(pwd)
elif [ $# -eq 2 ]; then
        dir=$2
else
        echo "Usage: $0 <uri> [filepath] "
        exit 1
fi
uri=$1
filename=$(basename "$uri")
status=-1
while (( status != 0 ))
do
        PIDS=$(pgrep aria2c)
        if [ -z "$PIDS" ]; then
                aria2c -d $dir -o $filename -s14 -x14 -k100M $uri
        fi
        status=$?
        pid=$(pidof aria2c)
        wait $pid
        echo aria2c exit.
        case $status in
                3)
                        echo file not exist.
                        exit 3
                        ;;
                9)
                        echo No space left on device.
                        exit 9
                        ;;
                *)
                        continue
                        ;;
        esac
done
echo download succeed.
exit 0
```

- Performance pretty good compare to `wget` command:

```
[#daede1 145GiB/145GiB(99%) CN:1 DL:115MiB]
10/05 10:34:40 [NOTICE] Download complete: /axie/geth.tar.zst

Download Results:
gid   |stat|avg speed  |path/URI
======+====+===========+=======================================================
daede1|OK  |   207MiB/s|/axie/geth.tar.zst

Status Legend:
(OK):download completed.

real    12m2.862s
user    1m57.320s
sys     2m28.624s
```

- Uncompress: `tar -I zstd -xvf chaindata.tar.zst`. It will take more than 20 min to uncompress. You can put it in the `tmux` session and run command `tar -I zst -xvf chaindata.tar.zst`
- You can combine the above steps by running a script:

```
wget -O chaindata.tar.zst  "<paste snapshot URL here>"
tar -I zstd -xvf chaindata.tar.zst
```


- If you do not need to store the archive for use with other nodes, you may also extract it while downloading to save time and disk space:
```
wget -q -O - <snapshot URL> | tar -I zstd -xvf -
```


Step 3: Install the node
- Now you can follow steps by steps from here [Install the node ](https://docs.roninchain.com/developers/nodes/mainnet)
- This docs is the detail for `4.(Optional) Download the snapshot`


### Chaindata snapshot - Archive Node (Leveldb)
#### Endpoint (Mainnet):

Storage size: 15T - we split it into 500GB for each file.


- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-000](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-000)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-001](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-001)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-002](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-002)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-003](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-003)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-004](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-004)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-005](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-005)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-006](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-006)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-007](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-007)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-008](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-008)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-009](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-009)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-010](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-010)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-011](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-011)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-012](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-012)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-013](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-013)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-014](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-014)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-015](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-015)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-016](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-016)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-017](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-017)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-018](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-018)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-019](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-019)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-020](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-020)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-021](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-021)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-022](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-022)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-023](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-023)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-024](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-024)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-025](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-025)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-026](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-026)

- [leveldb-archive-mainnet-chaindata-20250701.tar.zst-027](https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-027)




#### Usage
- Download && Concatenate && Uncompress:

```shell
for i in {000..027}; do wget "https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-$i"; done
cat "leveldb-archive-mainnet-chaindata-20250701.tar.zst-"* > chaindata.tar.zst
tar -I zstd -xvf chaindata.tar.zst
```

- If you do not need to store the archive for use with other nodes, you may also extract it while joining files to save time and disk space:

```shell
for i in {000..027}; do wget "https://ss.roninchain.com/leveldb-archive-mainnet-chaindata-20250701.tar.zst-$i"; done
cat "leveldb-archive-mainnet-chaindata-20250701.tar.zst-"* | tar -I zstd -xvf - -C chaindata
```
