# lab7.2
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'API Fetch Example',
      home: ApiDataPage(),
    );
  }
}

class ApiDataPage extends StatefulWidget {
  @override
  _ApiDataPageState createState() => _ApiDataPageState();
}

class _ApiDataPageState extends State<ApiDataPage> {
  Map<String, dynamic>? postData;
  bool isLoading = false;

  Future<void> fetchPost() async {
    setState(() {
      isLoading = true;
    });

    final response = await http
        .get(Uri.parse('https://jsonplaceholder.typicode.com/posts/1'));

    if (response.statusCode == 200) {
      setState(() {
        postData = json.decode(response.body);
        isLoading = false;
      });
    } else {
      setState(() {
        isLoading = false;
      });
      throw Exception('Failed to load post');
    }
  }

  @override
  void initState() {
    super.initState();
    fetchPost(); // fetch data on startup
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Fetch API Data'),
      ),
      body: Center(
        child: isLoading
            ? CircularProgressIndicator()
            : postData == null
                ? Text('No data fetched.')
                : Padding(
                    padding: const EdgeInsets.all(16.0),
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Text(
                          'Title:',
                          style: TextStyle(
                              fontWeight: FontWeight.bold, fontSize: 20),
                        ),
                        Text(postData!['title']),
                        SizedBox(height: 20),
                        Text(
                          'Body:',
                          style: TextStyle(
                              fontWeight: FontWeight.bold, fontSize: 20),
                        ),
                        Text(postData!['body']),
                      ],
                    ),
                  ),
      ),
    );
  }
}
