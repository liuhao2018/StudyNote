### Android文件路径

私有文件目录 应用卸载 文件将被删除
- context.getFileDir(); //  data/data/packagename/files
- context.getcacheDir(); // data/data/packagename/cache

与上面两种路径对应，在SDCard中，也会有两个类似的文件夹，但是不会随
应用的卸载被删除。

- context.getExternalFilesDir(); //SDCard/Android/data/packagename/files
- context.getExternalCacheDir(); //SDCard/Android/data/packagename/cache

- 获取SDCard根路径，即外部存储根路径： Enviroment.getExternalStorageDirectory().getAbsolutePath();