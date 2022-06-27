## Keygenme (200 pts)

Challenge này cung cấp cho ta một file thực thi ctf-challenge. Thử chạy file này:

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon1.png)

Nó bắt ta nhập vào một password và sẽ kiểm tra password đó. Ta sẽ mở file này trong ida xem sao.

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon2.png)

Tìm đến hàm main, ta thấy ở đây có một chuỗi mã hóa base64 và một giá trị băm SHA1 để check password. SHA1 thì không nên đụng tới rồi :v 
Nên ta sẽ thử giải mã chuỗi dài dài kia xem sao.

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon3.png)

Một gợi ý tuyệt vời. Vậy ta sẽ phải đi tìm tên một ban nhạc và tên một bài hát trong file này để tìm được password.
Xem lại các hàm của file, ta thấy có hai hàm lạ tên Get_Band_Name và Hint.

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon4-1.png)

Kiểm tra hàm Get_Band_Name:

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon4.png)

Ta thấy hàm này cho một vài giá trị, sau đó sẽ xor các giá trị này để thu về các giá trị nào đó khác? Vậy thì ta chỉ cần tiến hành xor các giá trị trên là được.

Kiểm tra hàm Hint:

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon5-1.png)

Cũng là rất nhiều giá trị lạ khác. Ở đây các giá trị có vẻ không khác nhau nhiều lắm. Vậy thì ở đây có thể là rotation cipher? 

Viết đoạn code sau để thử :v 

```
get_band_name = [(58, 119), (60, 89), (122, 14), (170, 203), (152, 244), (18, 126), (65, 40), (255, 156), (222, 191)]

# xor cac cap gia tri
for i in get_band_name:
	print(chr(i[0] ^ i[1]), end='')

print('\n')

hint = [157, 177, 195, 196, 181, 194, 112, 191, 182, 112, 160, 197, 192, 192, 181, 196, 195]

# 256 gia tri ASCII
for i in range(256):
	enc = ""
	for char in hint:
		enc += chr((char + i) % 256)
	print(enc)
```
Kết quả:

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon6.png)

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon7.png)

Vậy Band name là Metallca và tra trên wikipedia thì họ có một bài hát là Master of Puppets.

Password sẽ là 3 từ liên tiếp trong lời bài hát Master of Puppets: [Lyrics](https://genius.com/Metallica-master-of-puppets-lyrics)

3 từ này khả năng cao phải mang một ý nghĩa đầy đủ nào đó. Sau một vài lần thử thì thấy come_crawling_faster đưa ra kết quả success!

![](https://github.com/Kien-Ta/ctf_writeups/blob/main/TyphoonconCTF/2022/Typhoon8.png)

Vậy flag của challenge này: **come_crawling_faster**

