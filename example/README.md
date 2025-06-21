# Example

English | [中文](./README_CN.md)

## Read Data Directly

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

## Read Data Offset, Read Data Later

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

## Read Data After Stored Data Offset

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
