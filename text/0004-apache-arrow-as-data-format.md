- Start Date: 2023-02-17 14:30
- RFC PR:
- ArchGuard Issue: https://github.com/archguard/archguard/issues/97

# Summary

基于 Apache Arrow 格式完善：

- **数据序列化**：在 Client 侧，生成 Arrow 二进制数据，传输到 Server 侧。Server 侧，可直接使用，而无需序列化。
- **数据分析**：在 Plugin 侧，直接基于 Arrow 格式进行数据分析。
- **数据传输**：Arrow 比 JSON 更高效，更适合大数据量的传输。

# Basic example

Scanner CLI 输出的数据，可以直接用于 ArchGuard 的数据分析。

## CLI 转换示例

```kotlin
override fun saveDataStructure(codes: List<CodeDataStruct>) {
    codes.toDataFrame().writeArrowFeather(File(filename("codes")))
}

override fun saveApi(apis: List<ContainerService>) {
    apis.toDataFrame().writeArrowFeather(File(filename("apis")))
}
```

## 数据分析示例

如 Kotlin Dataframe
的示例：[https://kotlin.github.io/dataframe/overview.html](https://kotlin.github.io/dataframe/overview.html)

```kotlin
val df = DataFrame.read("titanic.csv", delimiter = ';')
// filter rows
df.filter { survived && home.endsWith("NY") && age in 10..20 }

// add column
df.add("birthYear") { 1912 - age }

// sort rows
df.sortByDesc { age }

// aggregate data
df.groupBy { pclass }.aggregate {
    maxBy { age }.name into "oldest person"
    count { survived } into "survived"
}
```

基于 Chapi 的示例：

```kotlin
val ds = listOf(
    CodeDataStruct(
        NodeName = "Main",
        Functions = listOf(CodeFunction(Name = "main")),
    )
)

val dataframe = ds.toDataFrame()

dataframe
    .filter { it[CodeDataStruct::Functions].isNotEmpty() }
    .filter { it[CodeDataStruct::Functions].any { function -> function.Name == "main" } }
    .print()
```

# Motivation

主要原因有：

1. 现有 issue 中：[文件数量多时 OutofMemomery](https://github.com/archguard/archguard/issues/97)
2. 通过 Arrow 格式，可以直接在 Plugin 侧进行数据分析，而无需序列化。

# Detailed design

针对于 Arrow 引入的改动：

- Scanner CLI 输出的数据，可以直接用于 ArchGuard 的数据分析。
    - 创建新的 `ArchGuardArrowClient`，用于生成 Arrow 格式的数据。
- Server 侧，接收到 Arrow 格式的数据，直接使用，而无需序列化。
    - 创建新的 `ArchGuardArrowServer`，用于接收 Arrow 格式的数据。
- Plugin 侧，直接基于 Arrow 格式进行数据分析。

针对于 OOM 需要变更的点：

- 在 `SourceCodeAnalyser` 改为 Coroutine，以支持大量数据的分析。
- 在 Scanner CLI 中，添加流式处理的功能，以支持大量数据的分析。
- 在 Server 侧，添加流式处理的功能，以支持大量数据的分析。

SourceCodeAnalyser 示例：

```kotlin
fun analyse(channel: Channel<CodeDataStruct>) = runBlocking {
    getFilesByPath(context.path) {
        it.absolutePath.endsWith(".go")
    }
        .map { async { analysisByFile(it) } }.awaitAll()
        .flatten()
        .forEach { channel.send(it) }
}
```

# Drawbacks

修改成本大，需要修改 Scanner CLI、Server、Plugin 等多个模块。

# Alternatives

类似于 Arrow 的格式，还有 Avro、Protobuf 等。

# Adoption strategy

# How we teach this

# Unresolved questions

Kotlin Dataframe 暂时不支持 nested type：https://github.com/Kotlin/dataframe/issues/271
