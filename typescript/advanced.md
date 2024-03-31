# Advanced modules

### [warp-core-worker](https://www.npmjs.com/package/warp-core-worker) - Task System for NodeJS
- Runs tasks on background
- Easily persisted to database (also can handle imports on start)
- Handlers re-tries and error handling
- very good to organize complex and multi step functionality
- tasks can be manual triggered, Cron or timeout based.

### [tachyon-drive](https://www.npmjs.com/package/tachyon-drive) - Common extendable object storage module
- Example usage: encrypted token cache, backend states for restarts, temporary cache storages.
- Implementation drivers for NodeJS "fs", S3, Azure Blob, Memcache
- Optional data processors: NodeJS AES cryption 
- Optional data validation
