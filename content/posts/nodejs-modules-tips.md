---
title: "Node.js 常用模块使用技巧：提升开发效率的实用指南"
date: 2026-05-03T01:01:24+08:00
draft: false
categories: ["技术教程", "Node.js"]
tags: ["Node.js", "后端开发", "JavaScript", "编程技巧", "模块开发"]
description: "深入解析 Node.js 常用核心模块的使用技巧，包括 fs、path、http、events、stream 等，分享最佳实践和性能优化方法，帮助开发者写出更高效、更可靠的 Node.js 代码。"
---

# Node.js 常用模块使用技巧：提升开发效率的实用指南

Node.js 作为基于 Chrome V8 引擎的 JavaScript 运行时，凭借其非阻塞 I/O 和事件驱动的特性，已成为构建高性能后端服务的首选平台。本文将深入探讨 Node.js 常用核心模块的使用技巧，帮助你写出更高效、更优雅的代码。

## 一、Path 模块：路径处理的正确姿势

### 1.1 基础用法与最佳实践

```javascript
const path = require('path');

// 路径拼接（跨平台兼容）
const configPath = path.join(__dirname, 'config', 'settings.json');
// Linux: /project/config/settings.json
// Windows: C:\project\config\settings.json

// 解析路径
const parsed = path.parse('/home/user/file.txt');
// { root: '/', dir: '/home/user', base: 'file.txt', ext: '.txt', name: 'file' }

// 规范化路径（处理 .. 和 .）
const normalized = path.normalize('/foo/bar/../baz/./test');
// '/foo/baz/test'

// 获取相对路径
const relative = path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
// '../../impl/bbb'
```

### 1.2 实用技巧

```javascript
// 技巧 1：安全地处理用户输入的路径
function safePath(userInput) {
    const basePath = process.cwd();
    const resolved = path.resolve(basePath, userInput);
    
    // 防止目录遍历攻击
    if (!resolved.startsWith(basePath)) {
        throw new Error('非法路径访问');
    }
    return resolved;
}

// 技巧 2：动态导入配置文件
function loadConfig(env = process.env.NODE_ENV) {
    const configPath = path.join(__dirname, 'config', `${env}.json`);
    return require(configPath);
}

// 技巧 3：统一文件扩展名处理
function ensureExtension(filePath, ext) {
    return path.extname(filePath) === ext ? filePath : `${filePath}${ext}`;
}

const jsFile = ensureExtension('./module', '.js'); // './module.js'
```

## 二、FS 模块：文件系统操作的高效方法

### 2.1 同步 vs 异步 vs Promise

```javascript
const fs = require('fs');
const fsPromises = require('fs').promises;

// ❌ 不推荐：阻塞主线程
try {
    const data = fs.readFileSync('file.txt', 'utf8');
} catch (err) {
    console.error(err);
}

// ✅ 推荐：回调方式
fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) {
        console.error(err);
        return;
    }
    console.log(data);
});

// ✅✅ 更推荐：async/await + Promise
async function readFileAsync() {
    try {
        const data = await fsPromises.readFile('file.txt', 'utf8');
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}
```

### 2.2 批量文件操作技巧

```javascript
const fsPromises = require('fs').promises;
const path = require('path');

// 技巧 1：并发读取多个文件（提升性能）
async function readMultipleFiles(filePaths) {
    const promises = filePaths.map(fp => 
        fsPromises.readFile(fp, 'utf8').catch(err => ({ error: err, file: fp }))
    );
    return await Promise.all(promises);
}

// 技巧 2：递归创建目录（Node.js 10+）
async function ensureDir(dirPath) {
    await fsPromises.mkdir(dirPath, { recursive: true });
}

// 技巧 3：安全写入文件（避免数据丢失）
async function safeWriteFile(filePath, content) {
    const tempPath = `${filePath}.tmp.${Date.now()}`;
    
    try {
        // 先写入临时文件
        await fsPromises.writeFile(tempPath, content, 'utf8');
        // 原子性重命名
        await fsPromises.rename(tempPath, filePath);
    } catch (err) {
        // 清理临时文件
        await fsPromises.unlink(tempPath).catch(() => {});
        throw err;
    }
}

// 技巧 4：流式大文件处理
const { createReadStream, createWriteStream } = require('fs');
const { pipeline } = require('stream');
const { promisify } = require('util');

const streamPipeline = promisify(pipeline);

async function copyLargeFile(source, dest) {
    await streamPipeline(
        createReadStream(source),
        createWriteStream(dest)
    );
}
```

### 2.3 文件监控的最佳实践

```javascript
const fs = require('fs');

// 使用 watchFile（轮询，兼容性好）
fs.watchFile('config.json', { interval: 1000 }, (curr, prev) => {
    if (curr.mtime !== prev.mtime) {
        console.log('配置文件已修改');
    }
});

// 使用 watch（事件驱动，更高效）
const watcher = fs.watch('config.json', (eventType, filename) => {
    if (eventType === 'change') {
        console.log(`${filename} 发生了变化`);
    }
});

// 记得在适当时候关闭监控
// watcher.close();
```

## 三、HTTP 模块：构建高效服务器

### 3.1 基础服务器搭建

```javascript
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
    // 解析 URL
    const parsedUrl = url.parse(req.url, true);
    
    // 设置响应头
    res.setHeader('Content-Type', 'application/json');
    res.setHeader('X-Powered-By', 'Node.js');
    
    // 路由处理
    if (parsedUrl.pathname === '/api/users' && req.method === 'GET') {
        res.statusCode = 200;
        res.end(JSON.stringify({ users: [] }));
    } else {
        res.statusCode = 404;
        res.end(JSON.stringify({ error: 'Not Found' }));
    }
});

server.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

### 3.2 请求体解析技巧

```javascript
// 手动解析 JSON 请求体
function parseJsonBody(req) {
    return new Promise((resolve, reject) => {
        let body = '';
        
        req.on('data', chunk => {
            body += chunk.toString();
            // 限制请求体大小，防止 DoS 攻击
            if (body.length > 1e6) {
                reject(new Error('Request body too large'));
            }
        });
        
        req.on('end', () => {
            try {
                resolve(JSON.parse(body));
            } catch (err) {
                reject(new Error('Invalid JSON'));
            }
        });
        
        req.on('error', reject);
    });
}

// 使用示例
const server = http.createServer(async (req, res) => {
    if (req.method === 'POST') {
        try {
            const data = await parseJsonBody(req);
            res.end(JSON.stringify({ received: data }));
        } catch (err) {
            res.statusCode = 400;
            res.end(JSON.stringify({ error: err.message }));
        }
    }
});
```

### 3.3 实现简单的中间件系统

```javascript
class MiddlewareServer {
    constructor() {
        this.middlewares = [];
    }
    
    use(middleware) {
        this.middlewares.push(middleware);
    }
    
    async handleRequest(req, res) {
        let index = 0;
        
        const next = async () => {
            if (index < this.middlewares.length) {
                const middleware = this.middlewares[index++];
                await middleware(req, res, next);
            }
        };
        
        try {
            await next();
        } catch (err) {
            res.statusCode = 500;
            res.end(JSON.stringify({ error: err.message }));
        }
    }
}

// 使用示例
const server = new MiddlewareServer();

// 日志中间件
server.use(async (req, res, next) => {
    console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`);
    await next();
});

// CORS 中间件
server.use(async (req, res, next) => {
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    await next();
});

// 业务逻辑
server.use(async (req, res) => {
    res.end(JSON.stringify({ message: 'Hello World' }));
});
```

## 四、Events 模块：事件驱动编程的艺术

### 4.1 EventEmitter 基础与进阶

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const emitter = new MyEmitter();

// 基础事件监听
emitter.on('event', (arg1, arg2) => {
    console.log('事件触发:', arg1, arg2);
});

emitter.emit('event', '参数 1', '参数 2');

// once：只执行一次
emitter.once('onceEvent', () => {
    console.log('只执行一次');
});

// prependListener：添加到队列开头
emitter.prependListener('priority', () => {
    console.log('优先执行');
});
```

### 4.2 异步事件处理

```javascript
// 异步事件监听器
emitter.on('asyncEvent', async (data) => {
    await someAsyncOperation(data);
});

// 等待事件发生（Node.js 11+）
const events = require('events');

async function waitForEvent(emitter, eventName) {
    const [data] = await events.once(emitter, eventName);
    return data;
}

// 使用示例
async function processData() {
    const result = await waitForEvent(emitter, 'dataReady');
    console.log('收到数据:', result);
}
```

### 4.3 错误处理最佳实践

```javascript
class DataProcessor extends EventEmitter {
    async process(data) {
        try {
            // 处理逻辑
            this.emit('progress', 50);
            await this.doWork(data);
            this.emit('progress', 100);
            this.emit('complete', data);
        } catch (err) {
            // 重要：异步错误必须通过事件或回调传递
            this.emit('error', err);
        }
    }
}

const processor = new DataProcessor();

// 必须监听 error 事件，否则会导致进程崩溃
processor.on('error', (err) => {
    console.error('处理失败:', err);
});

processor.on('complete', (result) => {
    console.log('处理完成:', result);
});
```

### 4.4 内存泄漏预防

```javascript
// 问题：无限增长的事件监听器
function badPractice() {
    const emitter = new EventEmitter();
    
    setInterval(() => {
        // 每次都会添加新的监听器
        emitter.on('event', handler);
    }, 1000);
}

// 解决方案 1：使用 once
function goodPractice1() {
    const emitter = new EventEmitter();
    
    setInterval(() => {
        emitter.once('event', handler);
    }, 1000);
}

// 解决方案 2：及时移除监听器
function goodPractice2() {
    const emitter = new EventEmitter();
    const handler = () => {};
    
    emitter.on('event', handler);
    
    // 在适当时机移除
    setTimeout(() => {
        emitter.off('event', handler);
    }, 5000);
}

// 设置最大监听器数量警告
emitter.setMaxListeners(20);
```

## 五、Stream 模块：处理大数据的利器

### 5.1 可读流的高级用法

```javascript
const { Readable } = require('stream');

// 自定义可读流
class NumberStream extends Readable {
    constructor(options) {
        super(options);
        this.current = 0;
        this.max = options.max || 100;
    }
    
    _read(size) {
        while (this.current < this.max) {
            if (!this.push(String(this.current++))) {
                return;
            }
        }
        this.push(null); // 结束流
    }
}

// 使用示例
const stream = new NumberStream({ max: 10 });
stream.on('data', (chunk) => {
    console.log('收到:', chunk.toString());
});
```

### 5.2 可写流的背压处理

```javascript
const { Writable } = require('stream');
const fs = require('fs');

class AsyncWritable extends Writable {
    async _write(chunk, encoding, callback) {
        try {
            // 模拟异步写入
            await new Promise(resolve => setTimeout(resolve, 100));
            console.log('写入:', chunk.toString());
            callback();
        } catch (err) {
            callback(err);
        }
    }
}

// 背压感知写入
async function writeWithBackPressure(stream, dataChunks) {
    for (const chunk of dataChunks) {
        if (!stream.write(chunk)) {
            // 等待 drain 事件
            await new Promise(resolve => {
                stream.once('drain', resolve);
            });
        }
    }
    stream.end();
}
```

### 5.3 Transform 流实战

```javascript
const { Transform } = require('stream');

// 创建转换流
class JSONParseStream extends Transform {
    constructor(options) {
        super({ ...options, objectMode: true });
        this.buffer = '';
    }
    
    _transform(chunk, encoding, callback) {
        this.buffer += chunk.toString();
        
        // 按行分割
        const lines = this.buffer.split('\n');
        this.buffer = lines.pop(); // 保留不完整的一行
        
        for (const line of lines) {
            if (line.trim()) {
                try {
                    this.push(JSON.parse(line));
                } catch (err) {
                    callback(err);
                    return;
                }
            }
        }
        callback();
    }
    
    _flush(callback) {
        if (this.buffer.trim()) {
            try {
                this.push(JSON.parse(this.buffer));
            } catch (err) {
                callback(err);
                return;
            }
        }
        callback();
    }
}

// 使用管道组合流
const { createReadStream, createWriteStream } = require('fs');
const { pipeline } = require('stream');
const { promisify } = require('util');

const streamPipeline = promisify(pipeline);

async function processLogFile(input, output) {
    await streamPipeline(
        createReadStream(input),
        new JSONParseStream(),
        new Transform({
            objectMode: true,
            transform(obj, enc, cb) {
                // 过滤和处理数据
                if (obj.level === 'error') {
                    this.push(JSON.stringify(obj) + '\n');
                }
                cb();
            }
        }),
        createWriteStream(output)
    );
}
```

### 5.4 Stream 性能优化技巧

```javascript
// 技巧 1：使用 highWaterMark 控制缓冲区大小
const readable = new Readable({
    highWaterMark: 1024, // 1KB
    read() {
        // ...
    }
});

// 技巧 2：并行处理流数据
const { PassThrough } = require('stream');

async function parallelStreamProcessing(inputStream, workerCount = 4) {
    const streams = Array.from({ length: workerCount }, () => new PassThrough());
    
    // 轮询分发
    let index = 0;
    inputStream.on('data', (chunk) => {
        streams[index++ % workerCount].write(chunk);
    });
    
    inputStream.on('end', () => {
        streams.forEach(s => s.end());
    });
    
    // 并行处理
    const results = await Promise.all(
        streams.map(stream => processStream(stream))
    );
    
    return results;
}
```

## 六、Util 模块：实用工具函数

### 6.1 格式化输出

```javascript
const util = require('util');

// 深度检查对象
const obj = { a: 1, b: { c: 2, d: [3, 4, 5] } };
console.log(util.inspect(obj, { depth: null, colors: true }));

// 格式化字符串
console.log(util.format('用户 %s 有 %d 个消息', 'Alice', 5));
// 用户 Alice 有 5 个消息

// 调试输出
console.debug(util.inspect(process.env, { depth: 1 }));
```

### 6.2 Promisify 化腐朽为神奇

```javascript
const util = require('util');
const fs = require('fs');

// 将回调函数转换为 Promise
const readFile = util.promisify(fs.readFile);
const writeFile = util.promisify(fs.writeFile);

// 自定义函数的 promisify
function customFunc(arg, callback) {
    setTimeout(() => {
        callback(null, `结果：${arg}`);
    }, 100);
}

const customFuncAsync = util.promisify(customFunc);

// 特殊符号定制 promisify 行为
const dns = require('dns');
const lookup = util.promisify.custom(dns.lookup, (hostname, family, callback) => {
    dns.lookup(hostname, family, (err, address, family) => {
        callback(err, { address, family });
    });
});
```

### 6.3 类型检查与继承

```javascript
const util = require('util');

// 类型检查
console.log(util.types.isNativeError(new Error())); // true
console.log(util.types.isPromise(Promise.resolve())); // true
console.log(util.types.isProxy(new Proxy({}, {}))); // true

// 继承辅助
const EventEmitter = require('events');

function MyEmitter() {
    EventEmitter.call(this);
}

util.inherits(MyEmitter, EventEmitter);

const emitter = new MyEmitter();
emitter.on('test', () => console.log('工作正常'));
```

## 七、Child Process 模块：子进程管理

### 7.1 exec vs spawn vs fork

```javascript
const { exec, spawn, fork } = require('child_process');

// exec：适合简单命令，返回完整输出
exec('ls -la', (error, stdout, stderr) => {
    if (error) {
        console.error(`执行错误：${error}`);
        return;
    }
    console.log(stdout);
});

// spawn：适合长时间运行的进程，流式处理
const ls = spawn('ls', ['-la']);

ls.stdout.on('data', (data) => {
    console.log(`输出：${data}`);
});

ls.stderr.on('data', (data) => {
    console.error(`错误：${data}`);
});

ls.on('close', (code) => {
    console.log(`子进程退出码：${code}`);
});

// fork：专用于 Node.js 模块，支持 IPC 通信
const child = fork('./worker.js');

child.send({ type: 'task', data: '处理这个' });

child.on('message', (msg) => {
    console.log('收到子进程消息:', msg);
});
```

### 7.2 进程池模式

```javascript
const { fork } = require('child_process');
const path = require('path');

class ProcessPool {
    constructor(scriptPath, size = 4) {
        this.workers = [];
        this.taskQueue = [];
        
        for (let i = 0; i < size; i++) {
            this.spawnWorker(scriptPath);
        }
    }
    
    spawnWorker(scriptPath) {
        const worker = fork(path.resolve(scriptPath));
        worker.ready = true;
        
        worker.on('message', (msg) => {
            worker.ready = true;
            this.processQueue();
        });
        
        worker.on('exit', () => {
            // 重启退出的 worker
            this.spawnWorker(scriptPath);
        });
        
        this.workers.push(worker);
    }
    
    execute(task) {
        return new Promise((resolve, reject) => {
            this.taskQueue.push({ task, resolve, reject });
            this.processQueue();
        });
    }
    
    processQueue() {
        if (this.taskQueue.length === 0) return;
        
        const worker = this.workers.find(w => w.ready);
        if (!worker) return;
        
        worker.ready = false;
        const { task, resolve, reject } = this.taskQueue.shift();
        
        const timeout = setTimeout(() => {
            reject(new Error('任务超时'));
            worker.kill();
        }, 30000);
        
        worker.once('message', (result) => {
            clearTimeout(timeout);
            resolve(result);
        });
        
        worker.send(task);
    }
}

// 使用示例
const pool = new ProcessPool('./worker.js', 4);

async function processTasks(tasks) {
    const results = await Promise.all(
        tasks.map(task => pool.execute(task))
    );
    return results;
}
```

## 八、Cluster 模块：多核 CPU 利用

### 8.1 基础集群配置

```javascript
const cluster = require('cluster');
const http = require('http');
const os = require('os');

if (cluster.isMaster) {
    const numCPUs = os.cpus().length;
    
    console.log(`主进程 ${process.pid} 启动`);
    
    // 根据 CPU 核心数创建工作进程
    for (let i = 0; i < numCPUs; i++) {
        cluster.fork();
    }
    
    cluster.on('exit', (worker, code, signal) => {
        console.log(`工作进程 ${worker.process.pid} 退出`);
        // 重启退出的工作进程
        cluster.fork();
    });
} else {
    // 工作进程共享服务器端口
    http.createServer((req, res) => {
        res.writeHead(200);
        res.end(`工作进程 ${process.pid} 处理请求\n`);
    }).listen(3000);
    
    console.log(`工作进程 ${process.pid} 启动`);
}
```

### 8.2 优雅重启策略

```javascript
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
    const workers = new Map();
    let isShuttingDown = false;
    
    function spawnWorker() {
        const worker = cluster.fork();
        workers.set(worker.id, { worker, createdAt: Date.now() });
    }
    
    // 初始化工作进程
    for (let i = 0; i < os.cpus().length; i++) {
        spawnWorker();
    }
    
    // 监听工作进程退出
    cluster.on('exit', (worker, code, signal) => {
        workers.delete(worker.id);
        
        if (!isShuttingDown) {
            console.log(`重启工作进程 ${worker.process.pid}`);
            spawnWorker();
        }
    });
    
    // 优雅重启所有工作进程
    function gracefulRestart() {
        isShuttingDown = true;
        
        const workerList = Array.from(workers.values());
        let restarted = 0;
        
        workerList.forEach(({ worker }) => {
            worker.disconnect();
            
            worker.on('disconnect', () => {
                worker.process.kill('SIGKILL');
                restarted++;
                
                if (restarted === workerList.length) {
                    isShuttingDown = false;
                    console.log('所有工作进程已重启');
                }
            });
        });
    }
    
    // 通过消息触发重启
    process.on('message', (msg) => {
        if (msg.type === 'restart') {
            gracefulRestart();
        }
    });
}
```

## 九、实际项目中的综合应用

### 9.1 构建高性能文件服务器

```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');
const { pipeline } = require('stream');
const { promisify } = require('util');
const crypto = require('crypto');

const streamPipeline = promisify(pipeline);

const MIME_TYPES = {
    '.html': 'text/html',
    '.css': 'text/css',
    '.js': 'application/javascript',
    '.json': 'application/json',
    '.png': 'image/png',
    '.jpg': 'image/jpeg',
};

class FileServer {
    constructor(rootDir, port = 3000) {
        this.rootDir = path.resolve(rootDir);
        this.port = port;
        this.server = null;
    }
    
    async start() {
        this.server = http.createServer((req, res) => {
            this.handleRequest(req, res).catch(err => {
                console.error(err);
                res.statusCode = 500;
                res.end('Internal Server Error');
            });
        });
        
        this.server.listen(this.port, () => {
            console.log(`文件服务器运行在 http://localhost:${this.port}`);
        });
    }
    
    async handleRequest(req, res) {
        // 解析 URL
        const urlPath = new URL(req.url, `http://${req.headers.host}`).pathname;
        
        // 安全检查：防止目录遍历
        const filePath = path.join(this.rootDir, urlPath);
        if (!filePath.startsWith(this.rootDir)) {
            res.statusCode = 403;
            res.end('Forbidden');
            return;
        }
        
        // 检查文件是否存在
        try {
            const stats = await fs.promises.stat(filePath);
            
            if (stats.isDirectory()) {
                // 尝试返回 index.html
                const indexPath = path.join(filePath, 'index.html');
                await this.serveFile(indexPath, res);
            } else {
                await this.serveFile(filePath, res);
            }
        } catch (err) {
            if (err.code === 'ENOENT') {
                res.statusCode = 404;
                res.end('Not Found');
            } else {
                throw err;
            }
        }
    }
    
    async serveFile(filePath, res) {
        const ext = path.extname(filePath).toLowerCase();
        const contentType = MIME_TYPES[ext] || 'application/octet-stream';
        
        // 生成 ETag
        const stats = await fs.promises.stat(filePath);
        const etag = `"${stats.size}-${stats.mtimeMs}"`;
        
        // 处理条件请求
        if (req.headers['if-none-match'] === etag) {
            res.statusCode = 304;
            res.end();
            return;
        }
        
        // 设置响应头
        res.setHeader('Content-Type', contentType);
        res.setHeader('Content-Length', stats.size);
        res.setHeader('ETag', etag);
        res.setHeader('Cache-Control', 'public, max-age=3600');
        
        // 流式传输文件
        await streamPipeline(
            fs.createReadStream(filePath),
            res
        );
    }
}

// 使用示例
const server = new FileServer('./public', 3000);
server.start();
```

### 9.2 日志系统实现

```javascript
const fs = require('fs');
const path = require('path');
const { Transform } = require('stream');
const EventEmitter = require('events');

class Logger extends EventEmitter {
    constructor(logDir = './logs', options = {}) {
        super();
        this.logDir = logDir;
        this.maxFileSize = options.maxFileSize || 10 * 1024 * 1024; // 10MB
        this.maxFiles = options.maxFiles || 5;
        this.currentStream = null;
        this.currentSize = 0;
        
        this.init();
    }
    
    async init() {
        await fs.promises.mkdir(this.logDir, { recursive: true });
        this.rotateLog();
    }
    
    getLogFileName() {
        const date = new Date().toISOString().split('T')[0];
        return path.join(this.logDir, `app-${date}.log`);
    }
    
    async rotateLog() {
        if (this.currentStream) {
            this.currentStream.end();
        }
        
        const logFile = this.getLogFileName();
        
        try {
            const stats = await fs.promises.stat(logFile);
            this.currentSize = stats.size;
        } catch {
            this.currentSize = 0;
        }
        
        this.currentStream = fs.createWriteStream(logFile, { flags: 'a' });
    }
    
    log(level, message, meta = {}) {
        const entry = {
            timestamp: new Date().toISOString(),
            level,
            message,
            ...meta
        };
        
        const line = JSON.stringify(entry) + '\n';
        
        if (this.currentStream) {
            this.currentStream.write(line);
            this.currentSize += Buffer.byteLength(line);
            
            // 检查是否需要轮转
            if (this.currentSize >= this.maxFileSize) {
                this.rotateLog();
            }
        }
        
        this.emit('log', entry);
        
        // 同时输出到控制台
        const color = this.getColor(level);
        console.log(`${color}[${entry.timestamp}] [${level}]${'\x1b[0m'} ${message}`);
    }
    
    getColor(level) {
        const colors = {
            error: '\x1b[31m', // 红
            warn: '\x1b[33m',  // 黄
            info: '\x1b[32m',  // 绿
            debug: '\x1b[36m'  // 青
        };
        return colors[level] || '\x1b[0m';
    }
    
    error(message, meta) { this.log('error', message, meta); }
    warn(message, meta) { this.log('warn', message, meta); }
    info(message, meta) { this.log('info', message, meta); }
    debug(message, meta) { this.log('debug', message, meta); }
}

// 使用示例
const logger = new Logger('./logs');

logger.info('服务器启动', { port: 3000 });
logger.error('数据库连接失败', { error: 'ECONNREFUSED' });

// 监听日志事件
logger.on('log', (entry) => {
    if (entry.level === 'error') {
        // 发送告警通知
        sendAlert(entry);
    }
});
```

## 十、性能优化与最佳实践总结

### 10.1 内存管理技巧

```javascript
// 1. 避免全局变量污染
(function() {
    // 模块级作用域
})();

// 2. 及时清理定时器
const timerId = setInterval(() => {}, 1000);
clearInterval(timerId); // 不需要时立即清除

// 3. 使用 WeakMap/WeakSet 存储临时引用
const cache = new WeakMap();

// 4. 监控内存使用
setInterval(() => {
    const usage = process.memoryUsage();
    console.log('内存使用:', {
        heapUsed: Math.round(usage.heapUsed / 1024 / 1024) + 'MB',
        heapTotal: Math.round(usage.heapTotal / 1024 / 1024) + 'MB'
    });
}, 10000);
```

### 10.2 错误处理黄金法则

```javascript
// 1. 始终处理 Promise 拒绝
async function safeOperation() {
    try {
        await riskyOperation();
    } catch (err) {
        // 记录错误并恢复
        console.error(err);
    }
}

// 2. 未捕获异常处理
process.on('uncaughtException', (err) => {
    console.error('未捕获异常:', err);
    // 优雅关闭
    process.exit(1);
});

process.on('unhandledRejection', (reason, promise) => {
    console.error('未处理的 Promise 拒绝:', reason);
});

// 3. 使用 domain 或 async_hooks 追踪异步错误上下文
const async_hooks = require('async_hooks');

const hook = async_hooks.createHook({
    init(asyncId, type, triggerAsyncId) {
        // 跟踪异步操作
    }
}).enable();
```

### 10.3 安全编码实践

```javascript
const crypto = require('crypto');

// 1. 密码哈希
function hashPassword(password) {
    const salt = crypto.randomBytes(16).toString('hex');
    const hash = crypto.pbkdf2Sync(password, salt, 100000, 64, 'sha512');
    return `${salt}:${hash.toString('hex')}`;
}

// 2. 防止命令注入
const { execFile } = require('child_process');
// ❌ 危险
// exec(`ls ${userInput}`);
// ✅ 安全
execFile('ls', [userInput]);

// 3. 安全的随机数生成
const secureToken = crypto.randomBytes(32).toString('hex');
```

## 总结

Node.js 的核心模块提供了强大的功能，掌握它们的使用技巧可以显著提升开发效率和代码质量：

1. **Path**：始终使用 `path.join` 和 `path.resolve` 保证跨平台兼容
2. **FS**：优先使用 `fs.promises` + `async/await`，大文件使用流
3. **HTTP**：注意请求体大小限制，实现背压处理
4. **Events**：及时清理监听器，避免内存泄漏
5. **Stream**：善用管道组合，处理大数据更高效
6. **Child Process**：根据场景选择 exec/spawn/fork
7. **Cluster**：充分利用多核 CPU，实现高可用

记住这些原则：
- ✅ 异步优先，避免阻塞
- ✅ 流式处理，节省内存
- ✅ 错误必捕，优雅降级
- ✅ 资源及时释放
- ✅ 安全编码，防患未然

持续学习和实践，你将能够编写出更加高效、可靠的 Node.js 应用程序！

---

**参考资源：**
- [Node.js 官方文档](https://nodejs.org/docs/latest/api/)
- [Node.js Design Patterns](https://www.nodejsdesignpatterns.com)
- [Stream Handbook](https://github.com/substack/stream-handbook)
