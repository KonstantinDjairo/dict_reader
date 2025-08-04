# Example

English | [中文](./README_CN.md)

## Locate

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  final offsetInfo = await dictReader.locate("go");
  print(await dictReader.readOneMdx(offsetInfo!));

  await dictReader.close();
}
```

## Search

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  final keys = dictReader.search("go");
  print(keys);

  final keysWithLimit = dictReader.search("go", limit: 1);
  print(keysWithLimit);

  await dictReader.close();
}
```

## Exist

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  final keyExists = dictReader.exist("go");
  print(keyExists);

  final keyDoesNotExist = dictReader.exist("non-existent-key");
  print(keyDoesNotExist);

  await dictReader.close();
}
```

## Read Data Directly

```dart
import 'package:dict_reader/dict_reader.dart';

void main() async {
  final dictReader = DictReader("MDX FILE PATH");
  await dictReader.init();

  await for (final MdxRecord(:keyText, :data) in dictReader.readWithMdxData()) {
    print("$keyText, $data");
  }

  await dictReader.close();
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

  await dictReader.close();
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

  await dictReader.close();
}
```