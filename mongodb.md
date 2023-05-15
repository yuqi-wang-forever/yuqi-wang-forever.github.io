```bash
docker compose up #启动两个容器
mongosh mongodb://localhost:27017 -u rootuser -p rootpass #链接mongosh 是一个js控制台


```
查看当前所有的数据库
```bash
For mongosh info see: https://docs.mongodb.com/mongodb-shell/
test> show dbs;
admin   100.00 KiB
config   60.00 KiB
local    72.00 KiB
```
localhost:8081 是mongo-express的管理界面nodejs写的
```bash
root@13c67dd083af:/# mongosh mongodb://localhost:2717 -u rootuser -p userpass
Current Mongosh Log ID: 646103dea7d2d2d4e8fb957a
Connecting to:          mongodb://<credentials>@localhost:2717/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.8.2
MongoNetworkError: connect ECONNREFUSED 127.0.0.1:2717
root@13c67dd083af:/# mongosh mongodb://localhost:27017 -u rootuser -p userpass
Current Mongosh Log ID: 646103e666473a372a7b169c
Connecting to:          mongodb://<credentials>@localhost:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.8.2
MongoServerError: Authentication failed.
root@13c67dd083af:/# mongodb mongodb://localhost:27017 -u rootuser -p rootpass
bash: mongodb: command not found
root@13c67dd083af:/# mongosh mongodb://localhost:27017 -u rootuser -p rootpass
Current Mongosh Log ID: 646104509fc1a847a8cc2bb4
Connecting to:          mongodb://<credentials>@localhost:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.8.2
Using MongoDB:          6.0.5
Using Mongosh:          1.8.2

For mongosh info see: https://docs.mongodb.com/mongodb-shell/
test> show dbs;
admin   100.00 KiB
config   60.00 KiB
local    72.00 KiB
test> use yuqicode;
switched to db yuqicode
yuqicode> show dbs;
admin   100.00 KiB
config  108.00 KiB
local    72.00 KiB
yuqicode> db.getName();
yuqicode
yuqicode> db.createCollection("Hello");
{ ok: 1 }
yuqicode> show dbs;
admin     100.00 KiB
config    108.00 KiB
local      72.00 KiB
yuqicode    8.00 KiB
yuqicode> db.dropDatabase();
{ ok: 1, dropped: 'yuqicode' }
yuqicode> db.help()

  Database Class:

    getMongo                                   Returns the current database connection
    getName                                    Returns the name of the DB
    getCollectionNames                         Returns an array containing the names of all collections in the current database.
    getCollectionInfos                         Returns an array of documents with collection information, i.e. collection name and options, for the current database.
    runCommand                                 Runs an arbitrary command on the database.
    adminCommand                               Runs an arbitrary command against the admin database.
    aggregate                                  Runs a specified admin/diagnostic pipeline which does not require an underlying collection.
    getSiblingDB                               Returns another database without modifying the db variable in the shell environment.
    getCollection                              Returns a collection or a view object that is functionally equivalent to using the db.<collectionName>.
    dropDatabase                               Removes the current database, deleting the associated data files.
    createUser                                 Creates a new user for the database on which the method is run. db.createUser() returns a duplicate user error if the user already exists on the database.
    updateUser                                 Updates the user’s profile on the database on which you run the method. An update to a field completely replaces the previous field’s values. This includes updates to the user’s roles array.
    changeUserPassword                         Updates a user’s password. Run the method in the database where the user is defined, i.e. the database you created the user.
    logout                                     Ends the current authentication session. This function has no effect if the current session is not authenticated.
    dropUser                                   Removes the user from the current database.
    dropAllUsers                               Removes all users from the current database.
    auth                                       Allows a user to authenticate to the database from within the shell.
    grantRolesToUser                           Grants additional roles to a user.
    revokeRolesFromUser                        Removes a one or more roles from a user on the current database.
    getUser                                    Returns user information for a specified user. Run this method on the user’s database. The user must exist on the database on which the method runs.
    getUsers                                   Returns information for all the users in the database.
    createCollection                           Create new collection
    createEncryptedCollection                  Creates a new collection with a list of encrypted fields each with unique and auto-created data encryption keys (DEKs). This is a utility function that internally utilises ClientEnryption.createEncryptedCollection.
    createView                                 Create new view
    createRole                                 Creates a new role.
    updateRole                                 Updates the role’s profile on the database on which you run the method. An update to a field completely replaces the previous field’s values.
    dropRole                                   Removes the role from the current database.
    dropAllRoles                               Removes all roles from the current database.
    grantRolesToRole                           Grants additional roles to a role.
    revokeRolesFromRole                        Removes a one or more roles from a role on the current database.
    grantPrivilegesToRole                      Grants additional privileges to a role.
    revokePrivilegesFromRole                   Removes a one or more privileges from a role on the current database.
    getRole                                    Returns role information for a specified role. Run this method on the role’s database. The role must exist on the database on which the method runs.
    getRoles                                   Returns information for all the roles in the database.
    currentOp                                  Runs an aggregation using $currentOp operator. Returns a document that contains information on in-progress operations for the database instance. For further information, see $currentOp.
    killOp                                     Calls the killOp command. Terminates an operation as specified by the operation ID. To find operations and their corresponding IDs, see $currentOp or db.currentOp().
    shutdownServer                             Calls the shutdown command. Shuts down the current mongod or mongos process cleanly and safely. You must issue the db.shutdownServer() operation against the admin database.
    fsyncLock                                  Calls the fsync command. Forces the mongod to flush all pending write operations to disk and locks the entire mongod instance to prevent additional writes until the user releases the lock with a corresponding db.fsyncUnlock() command.
    fsyncUnlock                                Calls the fsyncUnlock command. Reduces the lock taken by db.fsyncLock() on a mongod instance by 1.
    version                                    returns the db version. uses the buildinfo command
    serverBits                                 returns the db serverBits. uses the buildInfo command
    isMaster                                   Calls the isMaster command
    hello                                      Calls the hello command
    serverBuildInfo                            returns the db serverBuildInfo. uses the buildInfo command
    serverStatus                               returns the server stats. uses the serverStatus command
    stats                                      returns the db stats. uses the dbStats command
    hostInfo                                   Calls the hostInfo command
    serverCmdLineOpts                          returns the db serverCmdLineOpts. uses the getCmdLineOpts command
    rotateCertificates                         Calls the rotateCertificates command
    printCollectionStats                       Prints the collection.stats for each collection in the db.
    getFreeMonitoringStatus                    Calls the getFreeMonitoringStatus command
    disableFreeMonitoring                      returns the db disableFreeMonitoring. uses the setFreeMonitoring command
    enableFreeMonitoring                       returns the db enableFreeMonitoring. uses the setFreeMonitoring command
    getProfilingStatus                         returns the db getProfilingStatus. uses the profile command
    setProfilingLevel                          returns the db setProfilingLevel. uses the profile command
    setLogLevel                                returns the db setLogLevel. uses the setParameter command
    getLogComponents                           returns the db getLogComponents. uses the getParameter command
    cloneDatabase                              deprecated, non-functional
    cloneCollection                            deprecated, non-functional
    copyDatabase                               deprecated, non-functional
    commandHelp                                returns the db commandHelp. uses the passed in command with help: true
    listCommands                               Calls the listCommands command
    getLastErrorObj                            Calls the getLastError command
    getLastError                               Calls the getLastError command
    printShardingStatus                        Calls sh.status(verbose)
    printSecondaryReplicationInfo              Prints secondary replicaset information
    getReplicationInfo                         Returns replication information
    printReplicationInfo                       Formats sh.getReplicationInfo
    printSlaveReplicationInfo                  DEPRECATED. Use db.printSecondaryReplicationInfo
    setSecondaryOk                             This method is deprecated. Use db.getMongo().setReadPref() instead
    watch                                      Opens a change stream cursor on the database
    sql                                        (Experimental) Runs a SQL query against Atlas Data Lake. Note: this is an experimental feature that may be subject to change in future releases.

yuqicode> db.getMongo();
mongodb://<credentials>@localhost:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.8.2
yuqicode> db.createCollection("person")
{ ok: 1 }
yuqicode> show collections
person
yuqicode> db.person.status()
TypeError: db.person.status is not a function
yuqicode> db.person.stats()
{
  ok: 1,
  capped: false,
  wiredTiger: {
    metadata: { formatVersion: 1 },
    creationString: 'access_pattern_hint=none,allocation_size=4KB,app_metadata=(formatVersion=1),assert=(commit_timestamp=none,durable_timestamp=none,read_timestamp=none,write_timestamp=off),block_allocation=best,block_compressor=snappy,cache_resident=false,checksum=on,colgroups=,collator=,columns=,dictionary=0,encryption=(keyid=,name=),exclusive=false,extractor=,format=btree,huffman_key=,huffman_value=,ignore_in_memory_cache_size=false,immutable=false,import=(compare_timestamp=oldest_timestamp,enabled=false,file_metadata=,metadata_file=,repair=false),internal_item_max=0,internal_key_max=0,internal_key_truncate=true,internal_page_max=4KB,key_format=q,key_gap=10,leaf_item_max=0,leaf_key_max=0,leaf_page_max=32KB,leaf_value_max=64MB,log=(enabled=true),lsm=(auto_throttle=true,bloom=true,bloom_bit_count=16,bloom_config=,bloom_hash_count=8,bloom_oldest=false,chunk_count_limit=0,chunk_max=5GB,chunk_size=10MB,merge_custom=(prefix=,start_generation=0,suffix=),merge_max=15,merge_min=0),memory_page_image_max=0,memory_page_max=10m,os_cache_dirty_max=0,os_cache_max=0,prefix_compression=false,prefix_compression_min=4,readonly=false,source=,split_deepen_min_child=0,split_deepen_per_child=0,split_pct=90,tiered_object=false,tiered_storage=(auth_token=,bucket=,bucket_prefix=,cache_directory=,local_retention=300,name=,object_target_size=0),type=file,value_format=u,verbose=[],write_timestamp_usage=none',
    type: 'file',
    uri: 'statistics:table:collection-2--7179032034095206778',
    LSM: {
      'bloom filter false positives': 0,
      'bloom filter hits': 0,
      'bloom filter misses': 0,
      'bloom filter pages evicted from cache': 0,
      'bloom filter pages read into cache': 0,
      'bloom filters in the LSM tree': 0,
      'chunks in the LSM tree': 0,
      'highest merge generation in the LSM tree': 0,
      'queries that could have benefited from a Bloom filter that did not exist': 0,
      'sleep for LSM checkpoint throttle': 0,
      'sleep for LSM merge throttle': 0,
      'total size of bloom filters': 0
    },
    'block-manager': {
      'allocations requiring file extension': 0,
      'blocks allocated': 0,
      'blocks freed': 0,
      'checkpoint size': 0,
      'file allocation unit size': 4096,
      'file bytes available for reuse': 0,
      'file magic number': 120897,
      'file major version number': 1,
      'file size in bytes': 4096,
      'minor version number': 0
    },
    btree: {
      'btree checkpoint generation': 22,
      'btree clean tree checkpoint expiration time': 0,
      'btree compact pages reviewed': 0,
      'btree compact pages rewritten': 0,
      'btree compact pages skipped': 0,
      'btree skipped by compaction as process would not reduce size': 0,
      'column-store fixed-size leaf pages': 0,
      'column-store fixed-size time windows': 0,
      'column-store internal pages': 0,
      'column-store variable-size RLE encoded values': 0,
      'column-store variable-size deleted values': 0,
      'column-store variable-size leaf pages': 0,
      'fixed-record size': 0,
      'maximum internal page size': 4096,
      'maximum leaf page key size': 2867,
      'maximum leaf page size': 32768,
      'maximum leaf page value size': 67108864,
      'maximum tree depth': 0,
      'number of key/value pairs': 0,
      'overflow pages': 0,
      'row-store empty values': 0,
      'row-store internal pages': 0,
      'row-store leaf pages': 0
    },
    cache: {
      'bytes currently in the cache': 191,
      'bytes dirty in the cache cumulative': 0,
      'bytes read into cache': 0,
      'bytes written from cache': 0,
      'checkpoint blocked page eviction': 0,
      'checkpoint of history store file blocked non-history store page eviction': 0,
      'data source pages selected for eviction unable to be evicted': 0,
      'eviction gave up due to detecting an out of order on disk value behind the last update on the chain': 0,
      'eviction gave up due to detecting an out of order tombstone ahead of the selected on disk update': 0,
      'eviction gave up due to detecting an out of order tombstone ahead of the selected on disk update after validating the update chain': 0,
      'eviction gave up due to detecting out of order timestamps on the update chain after the selected on disk update': 0,
      'eviction gave up due to needing to remove a record from the history store but checkpoint is running': 0,
      'eviction walk passes of a file': 0,
      'eviction walk target pages histogram - 0-9': 0,
      'eviction walk target pages histogram - 10-31': 0,
      'eviction walk target pages histogram - 128 and higher': 0,
      'eviction walk target pages histogram - 32-63': 0,
      'eviction walk target pages histogram - 64-128': 0,
      'eviction walk target pages reduced due to history store cache pressure': 0,
      'eviction walks abandoned': 0,
      'eviction walks gave up because they restarted their walk twice': 0,
      'eviction walks gave up because they saw too many pages and found no candidates': 0,
      'eviction walks gave up because they saw too many pages and found too few candidates': 0,
      'eviction walks reached end of tree': 0,
      'eviction walks restarted': 0,
      'eviction walks started from root of tree': 0,
      'eviction walks started from saved location in tree': 0,
      'hazard pointer blocked page eviction': 0,
      'history store table insert calls': 0,
      'history store table insert calls that returned restart': 0,
      'history store table out-of-order resolved updates that lose their durable timestamp': 0,
      'history store table out-of-order updates that were fixed up by reinserting with the fixed timestamp': 0,
      'history store table reads': 0,
      'history store table reads missed': 0,
      'history store table reads requiring squashed modifies': 0,
      'history store table truncation by rollback to stable to remove an unstable update': 0,
      'history store table truncation by rollback to stable to remove an update': 0,
      'history store table truncation to remove an update': 0,
      'history store table truncation to remove range of updates due to key being removed from the data page during reconciliation': 0,
      'history store table truncation to remove range of updates due to out-of-order timestamp update on data page': 0,
      'history store table writes requiring squashed modifies': 0,
      'in-memory page passed criteria to be split': 0,
      'in-memory page splits': 0,
      'internal pages evicted': 0,
      'internal pages split during eviction': 0,
      'leaf pages split during eviction': 0,
      'modified pages evicted': 0,
      'overflow pages read into cache': 0,
      'page split during eviction deepened the tree': 0,
      'page written requiring history store records': 0,
      'pages read into cache': 0,
      'pages read into cache after truncate': 0,
      'pages read into cache after truncate in prepare state': 0,
      'pages requested from the cache': 0,
      'pages seen by eviction walk': 0,
      'pages written from cache': 0,
      'pages written requiring in-memory restoration': 0,
      'the number of times full update inserted to history store': 0,
      'the number of times reverse modify inserted to history store': 0,
      'tracked dirty bytes in the cache': 0,
      'unmodified pages evicted': 0
    },
    cache_walk: {
      'Average difference between current eviction generation when the page was last considered': 0,
      'Average on-disk page image size seen': 0,
      'Average time in cache for pages that have been visited by the eviction server': 0,
      'Average time in cache for pages that have not been visited by the eviction server': 0,
      'Clean pages currently in cache': 0,
      'Current eviction generation': 0,
      'Dirty pages currently in cache': 0,
      'Entries in the root page': 0,
      'Internal pages currently in cache': 0,
      'Leaf pages currently in cache': 0,
      'Maximum difference between current eviction generation when the page was last considered': 0,
      'Maximum page size seen': 0,
      'Minimum on-disk page image size seen': 0,
      'Number of pages never visited by eviction server': 0,
      'On-disk page image sizes smaller than a single allocation unit': 0,
      'Pages created in memory and never written': 0,
      'Pages currently queued for eviction': 0,
      'Pages that could not be queued for eviction': 0,
      'Refs skipped during cache traversal': 0,
      'Size of the root page': 0,
      'Total number of pages currently in cache': 0
    },
    'checkpoint-cleanup': {
      'pages added for eviction': 0,
      'pages removed': 0,
      'pages skipped during tree walk': 0,
      'pages visited': 0
    },
    compression: {
      'compressed page maximum internal page size prior to compression': 4096,
      'compressed page maximum leaf page size prior to compression ': 131072,
      'compressed pages read': 0,
      'compressed pages written': 0,
      'number of blocks with compress ratio greater than 64': 0,
      'number of blocks with compress ratio smaller than 16': 0,
      'number of blocks with compress ratio smaller than 2': 0,
      'number of blocks with compress ratio smaller than 32': 0,
      'number of blocks with compress ratio smaller than 4': 0,
      'number of blocks with compress ratio smaller than 64': 0,
      'number of blocks with compress ratio smaller than 8': 0,
      'page written failed to compress': 0,
      'page written was too small to compress': 0
    },
    cursor: {
      'Total number of entries skipped by cursor next calls': 0,
      'Total number of entries skipped by cursor prev calls': 0,
      'Total number of entries skipped to position the history store cursor': 0,
      'Total number of times a search near has exited due to prefix config': 0,
      'bulk loaded cursor insert calls': 0,
      'cache cursors reuse count': 0,
      'close calls that result in cache': 1,
      'create calls': 1,
      'cursor next calls that skip due to a globally visible history store tombstone': 0,
      'cursor next calls that skip greater than or equal to 100 entries': 0,
      'cursor next calls that skip less than 100 entries': 1,
      'cursor prev calls that skip due to a globally visible history store tombstone': 0,
      'cursor prev calls that skip greater than or equal to 100 entries': 0,
      'cursor prev calls that skip less than 100 entries': 0,
      'insert calls': 0,
      'insert key and value bytes': 0,
      modify: 0,
      'modify key and value bytes affected': 0,
      'modify value bytes modified': 0,
      'next calls': 1,
      'open cursor count': 0,
      'operation restarted': 0,
      'prev calls': 0,
      'remove calls': 0,
      'remove key bytes removed': 0,
      'reserve calls': 0,
      'reset calls': 2,
      'search calls': 0,
      'search history store calls': 0,
      'search near calls': 0,
      'truncate calls': 0,
      'update calls': 0,
      'update key and value bytes': 0,
      'update value size change': 0
    },
    reconciliation: {
      'approximate byte size of timestamps in pages written': 0,
      'approximate byte size of transaction IDs in pages written': 0,
      'dictionary matches': 0,
      'fast-path pages deleted': 0,
      'internal page key bytes discarded using suffix compression': 0,
      'internal page multi-block writes': 0,
      'leaf page key bytes discarded using prefix compression': 0,
      'leaf page multi-block writes': 0,
      'leaf-page overflow keys': 0,
      'maximum blocks required for a page': 0,
      'overflow values written': 0,
      'page checksum matches': 0,
      'page reconciliation calls': 0,
      'page reconciliation calls for eviction': 0,
      'pages deleted': 0,
      'pages written including an aggregated newest start durable timestamp ': 0,
      'pages written including an aggregated newest stop durable timestamp ': 0,
      'pages written including an aggregated newest stop timestamp ': 0,
      'pages written including an aggregated newest stop transaction ID': 0,
      'pages written including an aggregated newest transaction ID ': 0,
      'pages written including an aggregated oldest start timestamp ': 0,
      'pages written including an aggregated prepare': 0,
      'pages written including at least one prepare': 0,
      'pages written including at least one start durable timestamp': 0,
      'pages written including at least one start timestamp': 0,
      'pages written including at least one start transaction ID': 0,
      'pages written including at least one stop durable timestamp': 0,
      'pages written including at least one stop timestamp': 0,
      'pages written including at least one stop transaction ID': 0,
      'records written including a prepare': 0,
      'records written including a start durable timestamp': 0,
      'records written including a start timestamp': 0,
      'records written including a start transaction ID': 0,
      'records written including a stop durable timestamp': 0,
      'records written including a stop timestamp': 0,
      'records written including a stop transaction ID': 0
    },
    session: {
      'object compaction': 0,
      'tiered operations dequeued and processed': 0,
      'tiered operations scheduled': 0,
      'tiered storage local retention time (secs)': 0
    },
    transaction: {
      'race to read prepared update retry': 0,
      'rollback to stable history store records with stop timestamps older than newer records': 0,
      'rollback to stable inconsistent checkpoint': 0,
      'rollback to stable keys removed': 0,
      'rollback to stable keys restored': 0,
      'rollback to stable restored tombstones from history store': 0,
      'rollback to stable restored updates from history store': 0,
      'rollback to stable skipping delete rle': 0,
      'rollback to stable skipping stable rle': 0,
      'rollback to stable sweeping history store keys': 0,
      'rollback to stable updates removed from history store': 0,
      'transaction checkpoints due to obsolete pages': 0,
      'update conflicts': 0
    }
  },
  sharded: false,
  size: 0,
  count: 0,
  numOrphanDocs: 0,
  storageSize: 4096,
  totalIndexSize: 4096,
  totalSize: 8192,
  indexSizes: { _id_: 4096 },
  avgObjSize: 0,
  ns: 'yuqicode.person',
  nindexes: 1,
  scaleFactor: 1
}
yuqicode> db.person.drop()
true
yuqicode> show collections

yuqicode> db.createCollection("person",{ capped: true, size: 6142800, max: 3000})
{ ok: 1 }
yuqicode> db.person.stats()
{
  ok: 1,
  capped: true,
  max: 3000,
  wiredTiger: {
    metadata: { formatVersion: 1 },
    creationString: 'access_pattern_hint=none,allocation_size=4KB,app_metadata=(formatVersion=1),assert=(commit_timestamp=none,durable_timestamp=none,read_timestamp=none,write_timestamp=off),block_allocation=best,block_compressor=snappy,cache_resident=false,checksum=on,colgroups=,collator=,columns=,dictionary=0,encryption=(keyid=,name=),exclusive=false,extractor=,format=btree,huffman_key=,huffman_value=,ignore_in_memory_cache_size=false,immutable=false,import=(compare_timestamp=oldest_timestamp,enabled=false,file_metadata=,metadata_file=,repair=false),internal_item_max=0,internal_key_max=0,internal_key_truncate=true,internal_page_max=4KB,key_format=q,key_gap=10,leaf_item_max=0,leaf_key_max=0,leaf_page_max=32KB,leaf_value_max=64MB,log=(enabled=true),lsm=(auto_throttle=true,bloom=true,bloom_bit_count=16,bloom_config=,bloom_hash_count=8,bloom_oldest=false,chunk_count_limit=0,chunk_max=5GB,chunk_size=10MB,merge_custom=(prefix=,start_generation=0,suffix=),merge_max=15,merge_min=0),memory_page_image_max=0,memory_page_max=10m,os_cache_dirty_max=0,os_cache_max=0,prefix_compression=false,prefix_compression_min=4,readonly=false,source=,split_deepen_min_child=0,split_deepen_per_child=0,split_pct=90,tiered_object=false,tiered_storage=(auth_token=,bucket=,bucket_prefix=,cache_directory=,local_retention=300,name=,object_target_size=0),type=file,value_format=u,verbose=[],write_timestamp_usage=none',
    type: 'file',
    uri: 'statistics:table:collection-4--7179032034095206778',
    LSM: {
      'bloom filter false positives': 0,
      'bloom filter hits': 0,
      'bloom filter misses': 0,
      'bloom filter pages evicted from cache': 0,
      'bloom filter pages read into cache': 0,
      'bloom filters in the LSM tree': 0,
      'chunks in the LSM tree': 0,
      'highest merge generation in the LSM tree': 0,
      'queries that could have benefited from a Bloom filter that did not exist': 0,
      'sleep for LSM checkpoint throttle': 0,
      'sleep for LSM merge throttle': 0,
      'total size of bloom filters': 0
    },
    'block-manager': {
      'allocations requiring file extension': 0,
      'blocks allocated': 0,
      'blocks freed': 0,
      'checkpoint size': 0,
      'file allocation unit size': 4096,
      'file bytes available for reuse': 0,
      'file magic number': 120897,
      'file major version number': 1,
      'file size in bytes': 4096,
      'minor version number': 0
    },
    btree: {
      'btree checkpoint generation': 0,
      'btree clean tree checkpoint expiration time': 0,
      'btree compact pages reviewed': 0,
      'btree compact pages rewritten': 0,
      'btree compact pages skipped': 0,
      'btree skipped by compaction as process would not reduce size': 0,
      'column-store fixed-size leaf pages': 0,
      'column-store fixed-size time windows': 0,
      'column-store internal pages': 0,
      'column-store variable-size RLE encoded values': 0,
      'column-store variable-size deleted values': 0,
      'column-store variable-size leaf pages': 0,
      'fixed-record size': 0,
      'maximum internal page size': 4096,
      'maximum leaf page key size': 2867,
      'maximum leaf page size': 32768,
      'maximum leaf page value size': 67108864,
      'maximum tree depth': 0,
      'number of key/value pairs': 0,
      'overflow pages': 0,
      'row-store empty values': 0,
      'row-store internal pages': 0,
      'row-store leaf pages': 0
    },
    cache: {
      'bytes currently in the cache': 191,
      'bytes dirty in the cache cumulative': 0,
      'bytes read into cache': 0,
      'bytes written from cache': 0,
      'checkpoint blocked page eviction': 0,
      'checkpoint of history store file blocked non-history store page eviction': 0,
      'data source pages selected for eviction unable to be evicted': 0,
      'eviction gave up due to detecting an out of order on disk value behind the last update on the chain': 0,
      'eviction gave up due to detecting an out of order tombstone ahead of the selected on disk update': 0,
      'eviction gave up due to detecting an out of order tombstone ahead of the selected on disk update after validating the update chain': 0,
      'eviction gave up due to detecting out of order timestamps on the update chain after the selected on disk update': 0,
      'eviction gave up due to needing to remove a record from the history store but checkpoint is running': 0,
      'eviction walk passes of a file': 0,
      'eviction walk target pages histogram - 0-9': 0,
      'eviction walk target pages histogram - 10-31': 0,
      'eviction walk target pages histogram - 128 and higher': 0,
      'eviction walk target pages histogram - 32-63': 0,
      'eviction walk target pages histogram - 64-128': 0,
      'eviction walk target pages reduced due to history store cache pressure': 0,
      'eviction walks abandoned': 0,
      'eviction walks gave up because they restarted their walk twice': 0,
      'eviction walks gave up because they saw too many pages and found no candidates': 0,
      'eviction walks gave up because they saw too many pages and found too few candidates': 0,
      'eviction walks reached end of tree': 0,
      'eviction walks restarted': 0,
      'eviction walks started from root of tree': 0,
      'eviction walks started from saved location in tree': 0,
      'hazard pointer blocked page eviction': 0,
      'history store table insert calls': 0,
      'history store table insert calls that returned restart': 0,
      'history store table out-of-order resolved updates that lose their durable timestamp': 0,
      'history store table out-of-order updates that were fixed up by reinserting with the fixed timestamp': 0,
      'history store table reads': 0,
      'history store table reads missed': 0,
      'history store table reads requiring squashed modifies': 0,
      'history store table truncation by rollback to stable to remove an unstable update': 0,
      'history store table truncation by rollback to stable to remove an update': 0,
      'history store table truncation to remove an update': 0,
      'history store table truncation to remove range of updates due to key being removed from the data page during reconciliation': 0,
      'history store table truncation to remove range of updates due to out-of-order timestamp update on data page': 0,
      'history store table writes requiring squashed modifies': 0,
      'in-memory page passed criteria to be split': 0,
      'in-memory page splits': 0,
      'internal pages evicted': 0,
      'internal pages split during eviction': 0,
      'leaf pages split during eviction': 0,
      'modified pages evicted': 0,
      'overflow pages read into cache': 0,
      'page split during eviction deepened the tree': 0,
      'page written requiring history store records': 0,
      'pages read into cache': 0,
      'pages read into cache after truncate': 0,
      'pages read into cache after truncate in prepare state': 0,
      'pages requested from the cache': 0,
      'pages seen by eviction walk': 0,
      'pages written from cache': 0,
      'pages written requiring in-memory restoration': 0,
      'the number of times full update inserted to history store': 0,
      'the number of times reverse modify inserted to history store': 0,
      'tracked dirty bytes in the cache': 0,
      'unmodified pages evicted': 0
    },
    cache_walk: {
      'Average difference between current eviction generation when the page was last considered': 0,
      'Average on-disk page image size seen': 0,
      'Average time in cache for pages that have been visited by the eviction server': 0,
      'Average time in cache for pages that have not been visited by the eviction server': 0,
      'Clean pages currently in cache': 0,
      'Current eviction generation': 0,
      'Dirty pages currently in cache': 0,
      'Entries in the root page': 0,
      'Internal pages currently in cache': 0,
      'Leaf pages currently in cache': 0,
      'Maximum difference between current eviction generation when the page was last considered': 0,
      'Maximum page size seen': 0,
      'Minimum on-disk page image size seen': 0,
      'Number of pages never visited by eviction server': 0,
      'On-disk page image sizes smaller than a single allocation unit': 0,
      'Pages created in memory and never written': 0,
      'Pages currently queued for eviction': 0,
      'Pages that could not be queued for eviction': 0,
      'Refs skipped during cache traversal': 0,
      'Size of the root page': 0,
      'Total number of pages currently in cache': 0
    },
    'checkpoint-cleanup': {
      'pages added for eviction': 0,
      'pages removed': 0,
      'pages skipped during tree walk': 0,
      'pages visited': 0
    },
    compression: {
      'compressed page maximum internal page size prior to compression': 4096,
      'compressed page maximum leaf page size prior to compression ': 131072,
      'compressed pages read': 0,
      'compressed pages written': 0,
      'number of blocks with compress ratio greater than 64': 0,
      'number of blocks with compress ratio smaller than 16': 0,
      'number of blocks with compress ratio smaller than 2': 0,
      'number of blocks with compress ratio smaller than 32': 0,
      'number of blocks with compress ratio smaller than 4': 0,
      'number of blocks with compress ratio smaller than 64': 0,
      'number of blocks with compress ratio smaller than 8': 0,
      'page written failed to compress': 0,
      'page written was too small to compress': 0
    },
    cursor: {
      'Total number of entries skipped by cursor next calls': 0,
      'Total number of entries skipped by cursor prev calls': 0,
      'Total number of entries skipped to position the history store cursor': 0,
      'Total number of times a search near has exited due to prefix config': 0,
      'bulk loaded cursor insert calls': 0,
      'cache cursors reuse count': 0,
      'close calls that result in cache': 1,
      'create calls': 1,
      'cursor next calls that skip due to a globally visible history store tombstone': 0,
      'cursor next calls that skip greater than or equal to 100 entries': 0,
      'cursor next calls that skip less than 100 entries': 1,
      'cursor prev calls that skip due to a globally visible history store tombstone': 0,
      'cursor prev calls that skip greater than or equal to 100 entries': 0,
      'cursor prev calls that skip less than 100 entries': 0,
      'insert calls': 0,
      'insert key and value bytes': 0,
      modify: 0,
      'modify key and value bytes affected': 0,
      'modify value bytes modified': 0,
      'next calls': 1,
      'open cursor count': 0,
      'operation restarted': 0,
      'prev calls': 0,
      'remove calls': 0,
      'remove key bytes removed': 0,
      'reserve calls': 0,
      'reset calls': 2,
      'search calls': 0,
      'search history store calls': 0,
      'search near calls': 0,
      'truncate calls': 0,
      'update calls': 0,
      'update key and value bytes': 0,
      'update value size change': 0
    },
    reconciliation: {
      'approximate byte size of timestamps in pages written': 0,
      'approximate byte size of transaction IDs in pages written': 0,
      'dictionary matches': 0,
      'fast-path pages deleted': 0,
      'internal page key bytes discarded using suffix compression': 0,
      'internal page multi-block writes': 0,
      'leaf page key bytes discarded using prefix compression': 0,
      'leaf page multi-block writes': 0,
      'leaf-page overflow keys': 0,
      'maximum blocks required for a page': 0,
      'overflow values written': 0,
      'page checksum matches': 0,
      'page reconciliation calls': 0,
      'page reconciliation calls for eviction': 0,
      'pages deleted': 0,
      'pages written including an aggregated newest start durable timestamp ': 0,
      'pages written including an aggregated newest stop durable timestamp ': 0,
      'pages written including an aggregated newest stop timestamp ': 0,
      'pages written including an aggregated newest stop transaction ID': 0,
      'pages written including an aggregated newest transaction ID ': 0,
      'pages written including an aggregated oldest start timestamp ': 0,
      'pages written including an aggregated prepare': 0,
      'pages written including at least one prepare': 0,
      'pages written including at least one start durable timestamp': 0,
      'pages written including at least one start timestamp': 0,
      'pages written including at least one start transaction ID': 0,
      'pages written including at least one stop durable timestamp': 0,
      'pages written including at least one stop timestamp': 0,
      'pages written including at least one stop transaction ID': 0,
      'records written including a prepare': 0,
      'records written including a start durable timestamp': 0,
      'records written including a start timestamp': 0,
      'records written including a start transaction ID': 0,
      'records written including a stop durable timestamp': 0,
      'records written including a stop timestamp': 0,
      'records written including a stop transaction ID': 0
    },
    session: {
      'object compaction': 0,
      'tiered operations dequeued and processed': 0,
      'tiered operations scheduled': 0,
      'tiered storage local retention time (secs)': 0
    },
    transaction: {
      'race to read prepared update retry': 0,
      'rollback to stable history store records with stop timestamps older than newer records': 0,
      'rollback to stable inconsistent checkpoint': 0,
      'rollback to stable keys removed': 0,
      'rollback to stable keys restored': 0,
      'rollback to stable restored tombstones from history store': 0,
      'rollback to stable restored updates from history store': 0,
      'rollback to stable skipping delete rle': 0,
      'rollback to stable skipping stable rle': 0,
      'rollback to stable sweeping history store keys': 0,
      'rollback to stable updates removed from history store': 0,
      'transaction checkpoints due to obsolete pages': 0,
      'update conflicts': 0
    }
  },
  sharded: false,
  size: 0,
  count: 0,
  numOrphanDocs: 0,
  storageSize: 4096,
  totalIndexSize: 4096,
  totalSize: 8192,
  indexSizes: { _id_: 4096 },
  avgObjSize: 0,
  maxSize: 6142976,
  ns: 'yuqicode.person',
  nindexes: 1,
  scaleFactor: 1
}
yuqicode> db.person.drop()
true
yuqicode> show collections

yuqicode> db.createCollection("student")
{ ok: 1 }
yuqicode> show collections
student
yuqicode> db.createCollection("person")
{ ok: 1 }
yuqicode> show collections
person
student
yuqicode> db.student.drop()
true
yuqicode> db
yuqicode
yuqicode> student = {
...     "firstName": "Retha",
...     "lastName": "Killeen",
...     "email": "rkillen@mysql.com",
...     "gender": "F",
...     "country": "Philippines",
...     "isStudentActive": false,
...     "favouriteSubjects": [
...         "math",
...         "english",
...         "it"
...     ],
...     "totalSpentInBooks": 0.00
... }
{
  firstName: 'Retha',
  lastName: 'Killeen',
  email: 'rkillen@mysql.com',
  gender: 'F',
  country: 'Philippines',
  isStudentActive: false,
  favouriteSubjects: [ 'math', 'english', 'it' ],
  totalSpentInBooks: 0
}
yuqicode> db.student.insert(student)
DeprecationWarning: Collection.insert() is deprecated. Use insertOne, insertMany, or bulkWrite.
{
  acknowledged: true,
  insertedIds: { '0': ObjectId("64610b1a9fc1a847a8cc2bb5") }
}
yuqicode> show collections
person
student
yuqicode> db.student.find()
[
  {
    _id: ObjectId("64610b1a9fc1a847a8cc2bb5"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 0
  }
]
yuqicode> db.student.find().pretty()
[
  {
    _id: ObjectId("64610b1a9fc1a847a8cc2bb5"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 0
  }
]
yuqicode> students =
...     [
...         {
...             "firstName": "Retha",
...             "lastName": "Killeen",
...             "email": "rkillen@mysql.com",
...             "gender": "F",
...             "country": "Philippines",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "math",
...                 "english",
...                 "it"
...             ],
...             "totalSpentInBooks": 1000.00
...         },
...         {
...             "firstName": "Jack",
...             "lastName": "ONeill",
...             "email": "Jack@mysql.com",
...             "gender": "M",
...             "country": "USA",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "joke",
...                 "english",
...                 "it"
...             ],
...             "totalSpentInBooks": 1050.00
...         },
...         {
...             "firstName": "Sam",
...             "lastName": "Carter",
...             "email": "sam@mysql.com",
...             "gender": "F",
...             "country": "Canada",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "math",
...                 "english",
...                 "physical"
...             ],
...             "totalSpentInBooks": 3000.00
...         },
...         {
...             "firstName": "Daneil",
...             "lastName": "Jackson",
...             "email": "daneil@mysql.com",
...             "gender": "M",
...             "country": "USA",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "math",
...                 "english",
...                 "biology"
...             ],
...             "totalSpentInBooks": 2900.00
...         },
...         {
...             "firstName": "TealC",
...             "lastName": "TealC",
...             "email": "tealc@mysql.com",
...             "gender": "M",
...             "country": "Chulak",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "chinese",
...                 "english",
...                 "it"
...             ],
...             "totalSpentInBooks": 600.00
...         },
...         {
...             "firstName": "Bratac",
...             "lastName": "Bratac",
...             "email": "bratac@mysql.com",
...             "gender": "M",
...             "country": "Chulak",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "gym",
...                 "english",
...                 "it"
...             ],
...             "totalSpentInBooks": 4300.00
...         },
...         {
...             "firstName": "Geoge",
...             "lastName": "Hammond",
...             "email": "geoge@mysql.com",
...             "gender": "M",
...             "country": "USA",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "math",
...                 "english",
...                 "physical"
...             ],
...             "totalSpentInBooks": 5000.00
...         },{
...             "firstName": "Walter",
...             "lastName": "Wilson",
...             "email": "walter@mysql.com",
...             "gender": "F",
...             "country": "USA",
...             "isStudentActive": false,
...             "favouriteSubjects": [
...                 "math",
...                 "english",
...                 "program"
...             ],
...             "totalSpentInBooks": 1400.00
...         }
...     ]
[
  {
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 1000
  },
  {
    firstName: 'Jack',
    lastName: 'ONeill',
    email: 'Jack@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'joke', 'english', 'it' ],
    totalSpentInBooks: 1050
  },
  {
    firstName: 'Sam',
    lastName: 'Carter',
    email: 'sam@mysql.com',
    gender: 'F',
    country: 'Canada',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 3000
  },
  {
    firstName: 'Daneil',
    lastName: 'Jackson',
    email: 'daneil@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'biology' ],
    totalSpentInBooks: 2900
  },
  {
    firstName: 'TealC',
    lastName: 'TealC',
    email: 'tealc@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'chinese', 'english', 'it' ],
    totalSpentInBooks: 600
  },
  {
    firstName: 'Bratac',
    lastName: 'Bratac',
    email: 'bratac@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'gym', 'english', 'it' ],
    totalSpentInBooks: 4300
  },
  {
    firstName: 'Geoge',
    lastName: 'Hammond',
    email: 'geoge@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 5000
  },
  {
    firstName: 'Walter',
    lastName: 'Wilson',
    email: 'walter@mysql.com',
    gender: 'F',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'program' ],
    totalSpentInBooks: 1400
  }
]
yuqicode> db.student.insertMany(students)
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("64610f689fc1a847a8cc2bb6"),
    '1': ObjectId("64610f689fc1a847a8cc2bb7"),
    '2': ObjectId("64610f689fc1a847a8cc2bb8"),
    '3': ObjectId("64610f689fc1a847a8cc2bb9"),
    '4': ObjectId("64610f689fc1a847a8cc2bba"),
    '5': ObjectId("64610f689fc1a847a8cc2bbb"),
    '6': ObjectId("64610f689fc1a847a8cc2bbc"),
    '7': ObjectId("64610f689fc1a847a8cc2bbd")
  }
}
yuqicode> db.student.find().count()
9
yuqicode> db.student.find({firstName:'Jack'})
[
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    firstName: 'Jack',
    lastName: 'ONeill',
    email: 'Jack@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'joke', 'english', 'it' ],
    totalSpentInBooks: 1050
  }
]
yuqicode> db.student.find({firstName: 'Jack'},{firstName:1,lastName:2})
[
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    firstName: 'Jack',
    lastName: 'ONeill'
  }
]
yuqicode> db.student.find({firstName: 'Jack'},{firstName:1,lastName:5})
[
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    firstName: 'Jack',
    lastName: 'ONeill'
  }
]
yuqicode> db.student.find({firstName: 'Jack'},{firstName:1,lastName:0})
MongoServerError: Cannot do exclusion on field lastName in inclusion projection
yuqicode> db.student.find({firstName: 'Jack'},{firstName:1,lastName:1})
[
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    firstName: 'Jack',
    lastName: 'ONeill'
  }
]
yuqicode> db.student.find({firstName: 'Jack'},{firstName:0,lastName:0})
[
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    email: 'Jack@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'joke', 'english', 'it' ],
    totalSpentInBooks: 1050
  }
]
yuqicode> db.student.find({})
[
  {
    _id: ObjectId("64610b1a9fc1a847a8cc2bb5"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 0
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb6"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 1000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    firstName: 'Jack',
    lastName: 'ONeill',
    email: 'Jack@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'joke', 'english', 'it' ],
    totalSpentInBooks: 1050
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb8"),
    firstName: 'Sam',
    lastName: 'Carter',
    email: 'sam@mysql.com',
    gender: 'F',
    country: 'Canada',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 3000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb9"),
    firstName: 'Daneil',
    lastName: 'Jackson',
    email: 'daneil@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'biology' ],
    totalSpentInBooks: 2900
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bba"),
    firstName: 'TealC',
    lastName: 'TealC',
    email: 'tealc@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'chinese', 'english', 'it' ],
    totalSpentInBooks: 600
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbb"),
    firstName: 'Bratac',
    lastName: 'Bratac',
    email: 'bratac@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'gym', 'english', 'it' ],
    totalSpentInBooks: 4300
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbc"),
    firstName: 'Geoge',
    lastName: 'Hammond',
    email: 'geoge@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 5000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbd"),
    firstName: 'Walter',
    lastName: 'Wilson',
    email: 'walter@mysql.com',
    gender: 'F',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'program' ],
    totalSpentInBooks: 1400
  }
]
yuqicode> db.student.find({},{firstName:1,lastName:1,gender:1})
[
  {
    _id: ObjectId("64610b1a9fc1a847a8cc2bb5"),
    firstName: 'Retha',
    lastName: 'Killeen',
    gender: 'F'
yuqicode> db.student.update({_id:  ObjectId("64610f689fc1a847a8cc2bbd")},{$set:{'Casandra'}})
Uncaught:
SyntaxError: Unexpected token (1:80)

> 1 | db.student.update({_id:  ObjectId("64610f689fc1a847a8cc2bbd")},{$set:{'Casandra'}})
    |                                                                                 ^
  2 |

yuqicode> db.student.update({_id:  ObjectId("64610f689fc1a847a8cc2bbd")},{$set:{firstName: 'Casandra'}})
DeprecationWarning: Collection.update() is deprecated. Use updateOne, updateMany, or bulkWrite.
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
yuqicode> db.student.find()
[
  {
    _id: ObjectId("64610b1a9fc1a847a8cc2bb5"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 0
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb6"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 1000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    firstName: 'Jack',
    lastName: 'ONeill',
    email: 'Jack@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'joke', 'english', 'it' ],
    totalSpentInBooks: 1050
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb8"),
    firstName: 'Sam',
    lastName: 'Carter',
    email: 'sam@mysql.com',
    gender: 'F',
    country: 'Canada',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 3000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb9"),
    firstName: 'Daneil',
    lastName: 'Jackson',
    email: 'daneil@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'biology' ],
    totalSpentInBooks: 2900
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bba"),
    firstName: 'TealC',
    lastName: 'TealC',
    email: 'tealc@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'chinese', 'english', 'it' ],
    totalSpentInBooks: 600
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbb"),
    firstName: 'Bratac',
    lastName: 'Bratac',
    email: 'bratac@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'gym', 'english', 'it' ],
    totalSpentInBooks: 4300
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbc"),
    firstName: 'Geoge',
    lastName: 'Hammond',
    email: 'geoge@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 5000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbd"),
    firstName: 'Casandra',
    lastName: 'Wilson',
    email: 'walter@mysql.com',
    gender: 'F',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'program' ],
    totalSpentInBooks: 1400
  }
]
yuqicode> db.student.delete({_id: ObjectId("64610f689fc1a847a8cc2bbd")})
TypeError: db.student.delete is not a function
yuqicode> db.student.deleteOne({_id: ObjectId("64610f689fc1a847a8cc2bbd")})
{ acknowledged: true, deletedCount: 1 }
yuqicode> db.student.find()
[
  {
    _id: ObjectId("64610b1a9fc1a847a8cc2bb5"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 0
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb6"),
    firstName: 'Retha',
    lastName: 'Killeen',
    email: 'rkillen@mysql.com',
    gender: 'F',
    country: 'Philippines',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'it' ],
    totalSpentInBooks: 1000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb7"),
    firstName: 'Jack',
    lastName: 'ONeill',
    email: 'Jack@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'joke', 'english', 'it' ],
    totalSpentInBooks: 1050
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb8"),
    firstName: 'Sam',
    lastName: 'Carter',
    email: 'sam@mysql.com',
    gender: 'F',
    country: 'Canada',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 3000
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bb9"),
    firstName: 'Daneil',
    lastName: 'Jackson',
    email: 'daneil@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'biology' ],
    totalSpentInBooks: 2900
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bba"),
    firstName: 'TealC',
    lastName: 'TealC',
    email: 'tealc@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'chinese', 'english', 'it' ],
    totalSpentInBooks: 600
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbb"),
    firstName: 'Bratac',
    lastName: 'Bratac',
    email: 'bratac@mysql.com',
    gender: 'M',
    country: 'Chulak',
    isStudentActive: false,
    favouriteSubjects: [ 'gym', 'english', 'it' ],
    totalSpentInBooks: 4300
  },
  {
    _id: ObjectId("64610f689fc1a847a8cc2bbc"),
    firstName: 'Geoge',
    lastName: 'Hammond',
    email: 'geoge@mysql.com',
    gender: 'M',
    country: 'USA',
    isStudentActive: false,
    favouriteSubjects: [ 'math', 'english', 'physical' ],
    totalSpentInBooks: 5000
  }
]
yuqicode> db.student.find().count()
8
yuqicode> db.student.deleteMany({})
{ acknowledged: true, deletedCount: 8 }
yuqicode> db.student.find().count()
0
yuqicode>
```
