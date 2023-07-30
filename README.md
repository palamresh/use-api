# use-api

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class FirstPage extends StatefulWidget {
  @override
  State<StatefulWidget> createState() => _FirstPageState();
}

class _FirstPageState extends State {
  Future getData() async {
    String url = 'https://jsonplaceholder.typicode.com/photos';
    var data;

    var res = await http.get(Uri.parse(url));
    data = jsonDecode(res.body);
    return data;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text('Revision Flutter Course'),
        ),
        // body: data != null
        //     ? ListView.builder(
        //         itemCount: data.length,
        //         itemBuilder: (context, index) {
        //           return ListTile(
        //             leading: CircleAvatar(
        //               backgroundImage: NetworkImage(data[index]['url']),
        //             ),
        //             title: Text(data[index]['title']),
        //             subtitle: Text('ID ${data[index]['id']}'),
        //             trailing: Image.network(data[index]['thumbnailUrl']),
        //           );
        //         })
        //     : Center(
        //         child: CircularProgressIndicator(),
        //       ));
        body: FutureBuilder(
            future: getData(),
            builder: (context, snapshot) {
              switch (snapshot.connectionState) {
                case ConnectionState.none:
                  return Text('Fetch Something');

                case ConnectionState.active:
                case ConnectionState.waiting:
                  return Center(
                    child: CircularProgressIndicator(),
                  );
                case ConnectionState.done:
                  if (snapshot.hasError) {
                    return Text('Something is wrong');
                  }
              }
              return ListView.builder(
                  itemCount: snapshot.data.length,
                  itemBuilder: (context, index) {
                    return ListTile(
                      title: Text(snapshot.data[index]['title']),
                      leading: Image.network(snapshot.data[index]['url']),
                    );
                  });
            }));
  }
}
