# Flutter File Process

พัฒนาโดย 

[อาจาย์พิศาล สุขขี](https://www.facebook.com/numvarn)

phisan.s@sskru.ac.th

สาขาวิทยาการคอมพิวเตอร์, คณะศิลปศาสตร์และวิทยาศาสตร์ มหาวิทยาลัยราชภัฏศรีสะเกษ

**Packages we are using:**

- path_provider: [link](https://pub.dev/packages/path_provider)

**Description:**
โปรแกรมสำหรับแสดงการทำงานด้วยการประมวลผลไฟล์

โมบายแอปพลิเคชั่นนี้พัฒนาขึ้นเพื่อใช้เป็นสื่อการเรียนการสอน และตัวอย่างในกรณีศึกษาการพัฒนาโมบายแอปพลิเคชั่นด้วย Flutter ในรายวิชาการพัฒนาโปรแกรมบนมือถือ

เพื่อให้นักศึกษาได้ใช้สำหรับการศึกษา ทดลองปฏิบัติตาม ให้เกิดความรู้ ความเข้าใจ และทักษะในการพัฒนาโปรแกรมบนมือถือด้วย Flutter

## Class สำหรับการจัดการไฟล์

```dart
import 'dart:io';
import 'package:path_provider/path_provider.dart';

// Class for read/write User Todo json file
class DataFileProcess {
  Future<String> get _localPath async {
    final directory = await getApplicationDocumentsDirectory();
    return directory.path;
  }

  Future<File> get _localFile async {
    final path = await _localPath;
    return File('$path/data.json');
  }

  Future<String> readData() async {
    try {
      final file = await _localFile;
      String contents = await file.readAsString();
      return contents.toString();
    } catch (e) {
      return 'fail';
    }
  }

  Future<File> writeData(String data) async {
    final file = await _localFile;
    return file.writeAsString('$data');
  }
} //end class
```

## ตัวอย่างการใช้งาน
```dart
DataFileProcess dataFile = DataFileProcess();

// การอ่านข้อมูลจากไฟล์ในรูปแบบของสตริง
String dataStr = await dataFile.readData();

// การบันทึกข้อมูล String ลงในไฟล์
fileProcess.writeData('{}');
```

## ตัวอย่างการสร้าง JSON และบันทึกข้อมูลลงในไฟล์
```dart
DataFileProcess fileProcess = DataFileProcess();
List<Map> listData = [];

Map<String, dynamic> data = {
    'id': '0',
    'msg': 'Hello world',
};

listData.add(data);

// Convert List to json
var jsondata = jsonEncode(listData);

fileProcess.writeData(jsondata.toString());
```

## ตัวอย่างการลบรายการข้อมูลที่เลือกออกจากไฟล์

```dart
Future<void> _deleteMessage() async {
  // Remove element from list
  dataList.removeWhere((element) => element['id'] == selectedID.toString());

  // Convert List to json
  var jsondata = jsonEncode(dataList);
  if (jsondata.length != 0) {
    dataFile.writeData(jsondata.toString());
  } else {
    dataFile.writeData('{}');
  }

  Navigator.push(
      context,
      MaterialPageRoute(
          builder: (context) => MyHomePage(title: 'File Process')));
}
```