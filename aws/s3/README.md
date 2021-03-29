```
Make public
Canonical ID
```

```
🏃🏼‍♀️ Lab document
aws s3 ls  hiển thị list bucket
aws s3 ls s3://{buckname}/{path} dir bucket
aws s3 mb s3://{buckname}  tạo bucket
aws s3 rb s3://{buckname}   xóa bucket trường hợp bucket có data
aws s3 rb s3://{buckname} --force xóa bucket trường hợp bucket không có dữ liệu
aws s3 sync {folder path} s3://{buckname}/{path}   Đồng bộ lên bucket trường hợp này chỉ thêm và update mà không xóa
aws s3 sync {folder path} s3://{buckname}/{path} --delete Đồng bộ với thư mục local sẽ xóa file nếu không tồn tại ở local
aws s3 cp {file path} s3://{buckname}/{path}    copy file từ local lên s3
aws s3 mv {file path} s3://{buckname}/{path}    move file từ local lên s3
aws s3 rm s3://{buckname}/{file path} xóa file
aws s3 rm s3://{buckname}/{folder path} --recursive  xóa thư mụcaws s3 ls  hiển thị list bucket
aws s3 ls s3://{buckname}/{path} dir bucket
aws s3 mb s3://{buckname}  tạo bucket
aws s3 rb s3://{buckname}   xóa bucket trường hợp bucket có data
aws s3 rb s3://{buckname} --force xóa bucket trường hợp bucket không có dữ liệu
aws s3 sync {folder path} s3://{buckname}/{path}   Đồng bộ lên bucket trường hợp này chỉ thêm và update mà không xóa
aws s3 sync {folder path} s3://{buckname}/{path} --delete Đồng bộ với thư mục local sẽ xóa file nếu không tồn tại ở local
aws s3 cp {file path} s3://{buckname}/{path}    copy file từ local lên s3
aws s3 mv {file path} s3://{buckname}/{path}    move file từ local lên s3
aws s3 rm s3://{buckname}/{file path} xóa file
aws s3 rm s3://{buckname}/{folder path} --recursive  xóa thư mục
```
