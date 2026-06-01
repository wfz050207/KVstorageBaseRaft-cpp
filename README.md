# KVstorageBaseRaft-cpp

基于 Raft 共识算法的分布式键值存储系统，使用 C++20 实现。

## 项目简介

本项目实现了一个分布式、强一致性的键值存储数据库。核心采用 Raft 共识算法保证多节点间的数据一致性，上层使用跳表作为存储引擎，并自研了协程库与 RPC 通信框架作为底层支撑。

## 架构概览

```
┌─────────────────────────────┐
│         SkipList DB         │  ← 上层状态机（可替换）
├─────────────────────────────┤
│        RaftServer           │  ← Raft 与 DB 的协调层
├─────────────────────────────┤
│         Raft Core           │  ← 共识算法核心（选举、日志复制、持久化）
├─────────────────────────────┤
│     RPC Framework           │  ← 节点间通信
├─────────────────────────────┤
│     Fiber / Coroutine       │  ← 协程调度与异步 IO
└─────────────────────────────┘
```

## 模块说明

| 模块 | 路径 | 说明 |
|------|------|------|
| Raft 核心 | `src/raftCore/` | Raft 算法实现：领导者选举、日志复制、快照、持久化 |
| Raft 客户端 | `src/raftClerk/` | 集群客户端，向 Raft 集群发起读写请求 |
| RPC 框架 | `src/rpc/` | 自研 RPC 框架，基于 Protobuf + muduo |
| RPC 协议定义 | `src/raftRpcPro/` | Raft 通信所需的 Protobuf 定义 |
| 协程库 | `src/fiber/` | 用户态协程、IO 调度器、定时器 |
| 跳表 | `src/skipList/` | 内存键值存储引擎 |
| 公共库 | `src/common/` | 日志、配置工具 |

## 快速开始

### 依赖

- CMake >= 3.22
- GCC / Clang 支持 C++20
- [muduo](https://github.com/chenshuo/muduo) 网络库
- Protobuf

### 构建

```bash
mkdir cmake-build-debug && cd cmake-build-debug
cmake ..
make -j$(nproc)
```

生成的可执行文件位于 `bin/` 目录，库文件位于 `lib/` 目录。

### 运行示例

```bash
# Raft 集群 KV 数据库示例
./bin/raftKvDB

# RPC 调用示例
./bin/callFriendService
```

## 目录结构

```
.
├── src/            # 源代码
│   ├── common/     # 公共工具
│   ├── fiber/      # 协程库
│   ├── rpc/        # RPC 框架
│   ├── raftCore/   # Raft 核心
│   ├── raftClerk/  # Raft 客户端
│   ├── raftRpcPro/ # RPC Proto 定义
│   └── skipList/   # 跳表存储
├── example/        # 示例代码
├── test/           # 测试
├── docs/           # 文档
├── bin/            # 可执行文件输出
└── lib/            # 库文件输出
```

## 许可证

MIT License
