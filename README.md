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

