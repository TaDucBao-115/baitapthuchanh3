# baitapthuchanh3
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Tuần 02 Bài Tập',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const AgeCalculatorPage(),
    );
  }
}

class AgeCalculatorPage extends StatefulWidget {
  const AgeCalculatorPage({Key? key}) : super(key: key);

  @override
  _AgeCalculatorPageState createState() => _AgeCalculatorPageState();
}

class _AgeCalculatorPageState extends State<AgeCalculatorPage> {
  final _nameController = TextEditingController();
  final _ageController = TextEditingController();
  List<Map<String, dynamic>> members = [];
  String totalAgeResult = '';

  void calculateTotalAge() {
    int totalAge = 0;
    for (var member in members) {
      totalAge += member['age'] as int; // Explicitly cast to int
    }
    setState(() {
      totalAgeResult = 'Tổng tuổi của nhóm là: $totalAge';
    });
  }

  void addMember() {
    String name = _nameController.text.trim();
    int? age = int.tryParse(_ageController.text.trim()); // Use int? to handle null

    if (name.isNotEmpty && age != null && age >= 0) {
      setState(() {
        members.add({'name': name, 'age': age});
        _nameController.clear();
        _ageController.clear();
      });
      calculateTotalAge();
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('THỰC HÀNH Tuần 02'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            const Text(
              'Tạo nhóm và nhập thông tin thành viên',
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            TextField(
              controller: _nameController,
              decoration: const InputDecoration(
                labelText: 'Họ và tên',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 10),
            TextField(
              controller: _ageController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Tuổi',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 10),
            ElevatedButton(
              onPressed: addMember,
              child: const Text('Kiểm tra'),
            ),
            const SizedBox(height: 20),
            const Text(
              'Danh sách thành viên:',
              style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
            Expanded(
              child: ListView.builder(
                itemCount: members.length,
                itemBuilder: (context, index) {
                  return ListTile(
                    title: Text(members[index]['name']),
                    subtitle: Text('Tuổi: ${members[index]['age']}'),
                  );
                },
              ),
            ),
            Text(
              totalAgeResult,
              style: const TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
            ),
          ],
        ),
      ),
    );
  }
}
