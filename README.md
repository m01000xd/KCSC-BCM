# Lê Bảo Quân-AT190441-MB

## Reverse

### Real Warmup

Ném vào IDA:
![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/525d870c-e4a7-4e1a-8c0f-3c8520c0273d)

Chương trình bắt nhập vào flag, là 1 chuỗi kí tự, và ở dưới có hàm strcmp so khớp kí tự nhập vào với chuỗi: "S0NTQ3tDaDQwYF9NfF98bjlgX0QzTidfVjAxJ183N1wvX0tDU0N9"

Nhìn vào chuỗi trên có thể nó encode bằng base 64 hoặc 32. Ném chuỗi trên lên web decode trên google

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/ea907e2f-3fd6-46e5-81fd-982e7930b62c)


***Flag: KCSC{Ch40`_M|_|n9`_D3N'_V01'_77\/_KCSC}***

### hide and seek
![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/1e7c2126-f940-4685-a263-0350118dac5d)

Đề bài bảo khi chạy file dropFile.exe thì nó sẽ giấu 1 thứ gì đó ở trong máy.

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/ac9a5c82-1b3b-42f7-b4a3-a2e7cc0f72f4)

Bài này giống bài find me của năm ngoái, nó sẽ gen 1 file trong các folder user của mình, mà thường là ở Temp.

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/8c3e55fa-5847-43f2-b311-1e3d64d3a8e3)

Ở đây có file ‮temp_html_file_69013734.html
Thử mở lên:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/703ad433-8316-46f9-b90c-c9be87260927)

Chọn bên view flag:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/fecd9e76-24c0-438d-a24c-a7a0e9f2944c)

***Flag:KCSC{~(^._.)=^._.^=(._.^)~}***

### Images
![12](https://github.com/m01000xd/KCSC-BCM/assets/122852491/860baeb0-1879-43f6-9301-099318057814)
![11](https://github.com/m01000xd/KCSC-BCM/assets/122852491/60c433cd-e28b-4f47-81f2-fbec1f86053d)
![10](https://github.com/m01000xd/KCSC-BCM/assets/122852491/6656d21a-7901-4f73-a006-45378377495c)
![9](https://github.com/m01000xd/KCSC-BCM/assets/122852491/a9620da9-14ef-4887-a736-6692cf413cd0)
![8](https://github.com/m01000xd/KCSC-BCM/assets/122852491/2157f090-8bca-4cfa-b30b-a259391390c5)
![7](https://github.com/m01000xd/KCSC-BCM/assets/122852491/08db830f-8b79-425e-811d-b526b086ab55)
![6](https://github.com/m01000xd/KCSC-BCM/assets/122852491/46c7ed59-b3ed-423f-b7af-9d8680fe1fad)
![5](https://github.com/m01000xd/KCSC-BCM/assets/122852491/43a37db1-0c23-4011-acfd-9dcdcba0dbc2)
![4](https://github.com/m01000xd/KCSC-BCM/assets/122852491/2cc88bf8-1554-4462-97ab-7ac09f34f3af)
![3](https://github.com/m01000xd/KCSC-BCM/assets/122852491/efed78fd-ce46-437b-882f-cdc51376e7dd)
![2](https://github.com/m01000xd/KCSC-BCM/assets/122852491/0d822cfb-f4b3-4d44-b7ce-35b174100add)
![1](https://github.com/m01000xd/KCSC-BCM/assets/122852491/33a49053-091d-4123-889a-3c49c11185df)


Đề gồm 12 ảnh về 1 loạt code assembly.
Ta để ý điểm chung từ ảnh thứ 3 về cuối:
```
cmp eax, value
jnz short loc_140012834
```
và ảnh cuối, dòng cmp cuối cùng:
```
cmp eax, 67
jz short loc_140012849
```
```cmp eax, value``` là lệnh để so sánh giá trị được lưu trong thanh ghi eax với value. jnz và jz là các lệnh nhảy hàm sau lệnh cmp.
```jnz function``` là jump not zero, là lệnh nhảy khi cờ zero(ZF) được set, tức là nếu eax không = value, thì nó sẽ nhảy đến function được nói đến, ```jz function``` là jump zero, là lệnh ngược lại.

Từ ảnh 3 đến ảnh cuối, đều cmp eax với 1 giá trị và nếu eax không = giá trị đó thì sẽ nhảy đến hàm loc_140012834.

Tóm lại, chương trình của chúng ta sẽ kiểm tra các kí tự nhập vào, nếu không đúng, thì sẽ nhảy đến hàm thông báo sai flag.

Vậy việc bây giờ là gom hết các giá trị đem đi so với eax, đem ghép lại chính là flag.

Nhưng để ý kĩ thêm 1 chút, ở ngay dưới dòng cmp:

```
mov eax, 1
imul rax, value
movsx eax, [rbp+rax+230h+Buffer]
```
eax được set = 1. Sau đó rax = rax * value. '[rbp+rax+230h+Buffer]'- ở đây, Buffer chính là mảng kí tự mà chúng ta sẽ nhập vào. Mỗi ảnh rax sẽ có value khác nhau, hiểu nôm na dòng code cuối sẽ lưu giá trị so với eax vào địa chỉ rbp+rax+230h+Buffer.

Vì vậy, giờ ta có thứ tự của các kí tự flag trong bảng Unicode, và vị trí của chúng trong flag. Giờ cần sắp lại đúng thứ tự sẽ ra flag.

```python
lst = [75, 110, 101, 104, 101, 110, 107, 110, 95, 111, 67, 95, 110, 95, 118, 97, 67, 105, 121, 95, 104, 110, 109, 110, 104, 100, 111, 97, 110, 100, 104, 95, 95, 110, 97, 95, 116, 95, 105, 103, 110, 97, 95, 96, 125, 123, 105, 95, 97, 83, 67]

lst1 = [0, 0x1a, 0x14, 0x18, 34, 23, 18, 21, 22, 9, 5, 8, 10, 14, 12, 16, 3, 30, 48, 41, 44, 39, 7, 28, 29, 15, 38, 42, 46, 37, 33, 32, 36, 31, 25, 27, 35, 45, 19, 40,43,47,17,49,50, 4,13,11,6,2,1]
lst2 =([chr(x) for _,x in sorted(zip(lst1,lst))])
print(''.join(lst2))
```
***Flag:KCSC{Cam_on_vi_da_kien_nhan_nhin_het_dong_anh_nay`}***

### dynamic_function(bài này thi xong em mới giải ra)

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/d1c98a6a-3ac7-4811-9118-7e380258b69d)

Đầu tiên là ```fgets(Buffer, 32,v3)```, Buffer đọc 32 kí tự từ v3.

Xuống tới đây:

```  if ( j_strlen(Buffer) == 30 && (Str = (char *)sub_4113ED(Buffer)) != 0 && j_strlen(Str) == 24 )```
Đoạn này sẽ check độ dài Buffer phải = 30 và thêm yêu cầu về hàm ```sub_4113ED```:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/7cfe698c-b35a-4048-a832-aa9ba6623d18)

Hàm ```sub_4113ED``` sẽ dẫn đến 1 hàm khác, ```sub_411890```. Hàm này sẽ check coi Buffer có bắt đầu bằng ```KCSC{``` và kết thúc = ```}``` không và trả về đoạn chuỗi bị kẹp trong 2 dấu ngoặc {}.

Tóm lại, ```  if ( j_strlen(Buffer) == 30 && (Str = (char *)sub_4113ED(Buffer)) != 0 && j_strlen(Str) == 24 )``` sẽ check Buffer phải = 30, và đoạn kí tự ở trong Buffer.

Tiếp theo, ở dòng này:

```    lpAddress = (void (__cdecl *)(char *, char *, size_t))VirtualAlloc(0, 0xA4u, 0x1000u, 0x40u);```

Nhìn vào đây, thì có thể thấy lpAddress là 1 con trỏ hàm. Còn hàm VirtuallAlloc, là 1 hàm Win32, được sử dụng để cấp phát bộ nhớ ảo trong quá trình thực thi của một chương trình:
```
LPVOID VirtualAlloc(
  LPVOID lpAddress,
  SIZE_T dwSize,
  DWORD  flAllocationType,
  DWORD  flProtect
);

lpAddress: Địa chỉ bắt đầu của vùng bộ nhớ ảo cần cấp phát
dwSize: Kích thước của vùng bộ nhớ ảo cần cấp phát, tính bằng byte.
flAllocationType: Loại cấp phát.
flProtect: Quyền truy cập cho vùng bộ nhớ
```

Ở đây, hiểu đơn giản là địa chỉ của vùng nhớ mới sẽ được cấp phát vào lpAddress.

Tiếp theo, đoạn này:
```
    if ( lpAddress )
    {
      for ( i = 0; i < 0xA4; ++i )
        *((_BYTE *)lpAddress + i) = byte_417C50[i] ^ 0x41;
      v5 = j_strlen(Str);
      lpAddress(Str, v12, v5);
      VirtualFree(lpAddress, 0xA4u, 0x8000u);
```

Hiểu nôm na, trong vòng for thực hiện xor từng phần tử của mảng ```byte_417C50``` với 0x41, và lưu lần lượt theo thứ tự 4 byte 1 vào địa chỉ của con trỏ lpAdress. Với đoạn ```lpAdress(Str, v12, v5)``` khá khó hiểu, ta để yên ở đó. ```VirtualFree(lpAddress, 0xA4u, 0x8000u)``` sẽ giải phóng vùng nhớ lpAddress sau khi chạy xong đoạn code trước đó.

Ta lại xuống dưới:

```
      for ( j = 0; j < j_strlen(Str); ++j )
      {
        if ( v12[j] != byte_417CF4[j] )
          goto LABEL_18;
      }
      sub_4110D7("Not uncorrect ^_^", v7);
      return 0;
    }
    else
    {
      perror("VirtualAlloc failed");
      return 1;
    }
  }
  else
  {
LABEL_18:
    sub_4110D7("Not correct @_@", v7);
    return 0;
  }
}
```


Đoạn code sẽ kiểm tra xem từng kí tự trong ```v12``` có = từng kí tự trong mảng ```byte_417CF4``` không.Nếu đúng hết, thì flag của mình nhập là đúng.

Ở dòng này, ```      for ( j = 0; j < j_strlen(Str); ++j )```, chuột phải, chọn Synchronize with IDA View-A, rồi sang code assembly:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/af014a53-8c5b-4463-9860-4724219ffacf)

Đặt breakpoint ở đoạn ```cmp [ebp+var_70], eax```, sau đó F9 để debug, nhập bừa flag:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/0f59b032-b51e-4cc1-b3b6-141683d233df)

```v12``` của chúng ta trả về 1 chuỗi kí tự không khớp với các kí tự trong mảng cần so sánh. Tự hỏi rằng tại sao nó lại trả về như vậy ?

Quay lại với đoạn ```lpAdress(Str, v12, v5)```, khi ta đặt breakpoint ở đây, v12 lại ra 1 chuỗi kí tự khác. F7 để vào xem bên trong hàm này có gì:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/dce383bc-c2f5-468d-913a-9f218dc97d3a)

Chuột phải, Creat function, rồi F5:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/1c25fa8b-2167-4835-bb62-9fc522502068)


Đổi lại tên hàm(lpAdress),biến(Str,v12,v5) và hide cast:

![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/ab25dbc8-dfca-4186-a4c7-03f2fe64a4a9)

Ở đây, v4 chính là chuỗi ```'reversing_is_pretty_cool``` ,*&v4[13] có thể xem như 1 biến đếm i, với i ban đầu được set = 0. Đoạn code trên có thể được sửa lại bằng python như sau:

```python
i = 0
while (i < 24):
	v4[12] = 16 *(ord(Str[i]) % 16) + ord(Str[i]) // 16
	v12[i] = v4[12] ^ v4[i]
	i += 1
```

Như vậy, ta đã hiểu tại sao khi đặt breakpoint ở 2 dòng khác nhau, ```v12``` lại trả về các kết quả khác nhau.

Bây giờ, các kí tự trong ```v12``` phải chính là các kí tự trong mảng đem đi check. Sau đó mình đem đi xor lại sẽ ra được 1 giá trị riêng, với mỗi giá trị đó chỉ có 1 kí tự flag khớp: ```16*(ord(Str[i]) % 16) + ord(Str[i]) // 16```. Tạo 1 xâu rỗng rồi cộng dồn, sẽ ra được flag:

```python
v4 = 'reversing_is_pretty_cool'
v12 = [0x44, 0x93, 0x51, 0x42, 0x24, 0x45, 0x2E, 0x9B, 0x01, 0x99, 0x7F, 0x05, 0x4D, 0x47, 0x25, 0x43, 0xA2, 0xE2, 0x3E, 0xAA, 0x85, 0x99, 0x18, 0x7E]
v4 = [ord(i) for i in v4]
Str1 = ''
for j in range(24):
	a = v12[j] ^ v4[j]
	for i in range(256):
		if (16 *(i % 16) + i//16) == a:
			Str1 += chr(i)
flag = "KCSC{" + Str1 + "}"
print(flag)
```

Ở đây, vì flag của chúng ta luôn nằm trong 256 kí tự trong bảng mã ASCII, ta có thể check bằng cách brute force như vậy.

***Flag:KCSC{correct_flag!submit_now!}***

### Awg Mah Back(bài này thi xong e mới giải ra)

Bài này đổi lại file đọc từ file flag thành file output rồi thay biến flag = biến enc sẽ ra flag:

```python
from pwn import *
import base64
with open('output.txt', 'rb') as (f):
    enc = f.read()
a_enc = enc[0:len(enc) // 3]
b_enc = enc[len(enc) // 3:2 * len(enc) // 3]
c_enc = enc[2* len(enc) // 3:]
c_step1 = xor(c_enc, int(str(len(enc))[0]) * int(str(len(enc))[1]))
c_step2 = xor(b_enc, c_step1)
b_step = xor(a_enc, b_enc)
a_step = xor(c_step2, a_enc)
c_flag = xor(b_step, c_step2)
b_flag = xor(a_step, b_step)
a_flag = xor(a_step , int(str(len(enc))[0]) + int(str(len(enc))[1]))
flag = a_flag + b_flag + c_flag
for i in range(3):
    flag = base64.b64decode(flag)
print(flag.decode('ascii'))    
##with open('flag.txt', 'wb') as (f):
##    f.write(flag)
```
***Flag:KCSC{84cK_t0_BaCK_To_B4ck_X0r`_4nD_864_oM3g4LuL}***









## Crypto

### Ez_Ceasar

Bài này nếu để ý, 4 kí tự đầu của ct là ```l```,```d```,```t```,```d```. Và 4 kí tự đầu của flag là ```K```,```C```,```S```,```C```.
4 kí tự này đều cách nhau 42 kí tự trong mảng chuỗi alphabet của chúng ta, thực chất key = random.randint(0, 2**256) chỉ để đánh lừa chúng ta rằng key có thể sẽ rât lớn, nhưng thực ra, key=42, vì thực chất, khoảng cách của các kí tự của cipher và flag theo thứ tự lần lượt luôn không đổi.

Có được key, việc tìm flag rất đơn giản:
```python
ct='ldtdMdEQ8F7NC8Nd1F88CSF1NF3TNdBB1O'
key = 42

flag = ''
for k in ct:
    for i in alphabet:
        if alphabet[(alphabet.index(i) + 42) % 67] == k:
            flag += i
print(flag)
```

***Flag:KCSC{C3as4r_1s_Cl4ss1c4l_4nd_C00l}***

### Ceasar_but_Harder!!!!

Đối với bài này, ta để ý đến đây:
```python
for i in range(3):
   k = random.randint(0, alphabet.index('3'))
   alphabet = alphabet[:k] + alphabet[k+1:]
```
Hiểu đơn giản đoạn code trên sẽ xóa đi 3 kí tự trong chuỗi alphabet ban đầu. Ta không biết nó sẽ xóa đi 3 kí tự nào, nhưng ta biết rằng,
nó chắc chắn sẽ không xóa đi kí tự nào có mặt trong ct, cùng với ```K```,```C```,```S```,```C```,```{```,```}```

Do vậy, ý tưởng là tìm tất cả các trường hợp xóa đi 3 kí tự mà ngoại trừ các kí tự vừa nêu ở trên. Ta sẽ dùng itertools để liệt kê, sau đó vì khi đã xóa đi 3 kí tự, thì bài này sẽ trở thành bài Ez_ceasar, với vì thứ tự các chữ cái của cipher và flag không đổi, chỉ khác là có thêm nhiều trường hợp để ta lọc hơn.

```python
import string
from itertools import combinations

# Khai báo chuỗi alphabet
alphabet = string.ascii_letters + string.digits + "!{_}?"

# Ký tự được chỉ định không bị xóa
specified_chars = {'0', 's', '8', '9', 'c', 'j', 'm', 'p', 'T', 'G', 'q', 'M', 'n', 'N', 'h', 'o', '4', '}', 'g', 'v', 'f', 'R', 'V', '2','K','C','S', '{','_','5','3'}

# Tìm tất cả các kết hợp 3 ký tự cần xóa
chars_to_remove = set(alphabet) - specified_chars
combinations_to_remove = combinations(chars_to_remove, 3)
count = 0
# Liệt kê tất cả các trường hợp sau khi xóa 3 ký tự
lst = []
ct='2V9VnRcNosvgMo4RoVfThg8osNjo0G}mmqmp'

for combo in combinations_to_remove:
        modified_alphabet = ''.join(char for char in alphabet if char not in combo)
        if (modified_alphabet.index('2') - modified_alphabet.index('K')) == (modified_alphabet.index('V') - modified_alphabet.index('C')) == (modified_alphabet.index('9') - modified_alphabet.index('S')):
                lst.append(modified_alphabet)



for i in lst:
        flag = ''
        for k in ct:
                for j in i:
                        if i[(i.index(j) + 17) % 64] == k:
                            flag += j
        print(flag)

```
***Flag:KCSC{y0u_be4t_My_C3A54R_bu7_HoW!!?!}***

## Pwn

### A gift for pwners

Ném lên IDA, Shift+F12 để xem các string có trong file:
![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/fbe00cce-e557-49b1-a534-407f4397175f)
![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/e54b2c56-0cdb-4e9c-8529-d036258224b4)
![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/46e3472b-4f5a-442f-a0dd-eb40ae020f69)
![image](https://github.com/m01000xd/KCSC-BCM/assets/122852491/fd30148e-9cfb-4750-8160-306265d13367)

***Flag:KCSC{A_gift_for_the_pwners_0xdeadbeef}***

