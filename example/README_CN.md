# 示例

[English](./README.md) | 中文

## 定位

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  final offsetInfo = await dictReader.locate("go");
  print(await dictReader.readOneMdx(offsetInfo!));
}
```

## 搜索

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  final keys = dictReader.search("go");
  print(keys);

  final keysWithLimit = dictReader.search("go", limit: 1);
  print(keysWithLimit);
}
```

## 是否存在

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  final keyExists = dictReader.exist("go");
  print(keyExists);

  final keyDoesNotExist = dictReader.exist("non-existent-key");
  print(keyDoesNotExist);
}
```

## 直接读取数据

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  await for (final MdxRecord(:keyText, :data) in dictReader.readWithMdxData()) {
    print("$keyText, $data");
  }
}
```

## 读取数据 offset，之后读取数据

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  final map = <String, RecordOffsetInfo>{};
  await for (final offsetInfo in dictReader.readWithOffset()) {
    map[offsetInfo.keyText] = offsetInfo;
  }

  final offsetInfo = map["go"];
  print(await dictReader.readOneMdx(offsetInfo!));
}
```

## 当已保存数据 offset，读取数据

```dart
import 'package:dict_reader/dict_reader.dart';

// ...

void main() async {
  // ...

  final dictReader = DictReader("MDX FILE PATH");
  // Pass false to reduce initialization time
  await dictReader.init(false);

  final offsetInfo = map["go"];
  print(await dictReader.readOneMdx(offsetInfo!));
}
```