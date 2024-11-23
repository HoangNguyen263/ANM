# Tìm hiểu về RSA

Diffile và Hellman đã giới thiệu một cách tiếp cận mới với mật mã học và từ đó đã đưa ra yêu cầu phải có một thuật toán mật mã đáp ứng các yêu cầu đối với hệ thống khoá công khai. Một trong những thuật toán đầu tiên thành công trong việc đáp ứng lại yêu cầu đó đã được phát triển vào năm 1977 bởi Ron Rivest, Adi Shamir, và Len Adleman ở MIT và được xuất bản vào năm 1978. Thuật toán Rivest-Shamir-Adleman (RSA) đã trở thành chuẩn mật mã bất thành văn đối với PKC, cung cấp đảm bảo tính mật, xác thực và chữ ký điện tử.

Cơ sở thuật toán RSA dựa trên tính khó của bài toán phân tích các số lớn ra thừa số nguyên tố: không tồn tại thuật toán thời gian đa thức (theo độ dài của biểu diễn nhị phân của số đó)  cho bài toán này. Chẳng hạn, việc phân tích một hợp số là tích của 2 số nguyên tố lớn hàng  trăm chữ số sẽ mất hàng ngàn năm tính toán với một máy PC trung bình có CPU khoảng trên 2Ghz.

# Ý tưởng
Các nhà phát minh có lựa chọn khá giản dị là xây dựng thuật toán sinh/giải mã trên cơ sở phép toán lấy luỹ thừa đồng dư trên trường Zn = {0,1,2,..n-1}. Chẳng hạn, việc sinh mã cho tin X sẽ được thực hiện qua:
>Y = $X^6 \pm n$

Ở đây ta dùng ký hiệu $a = b \pm n$ nghĩa là $a = b \pm k* n$ với $a \in Z_n$ còn k = 1,2,3,..., ví dụ $7  = 3^3 \pm 10$ còn việc giải mã:
>X = $Y^d \pm n$

(*e*– khóa sinh mã, *d* – khóa giải mã)

Như vậy để hai hàm sinh mã và giải mã này là hàm ngược của nhau, e và d phải được chọn  
sao cho: $X^{ed} =Y^d \pm n$

Người ta đã tìm được cách xây dựng cặp số (e,d) này trên cơ sở công thức như sau: $X^{\phi (n)} = 1 \pm n$ (định lý Ơ - le).
Trong đó $\phi(n)$ hàm số cho biết số lượng các số thuộc $Z_n$ mà nguyên tố cùng nhau với n.  Người ta cần chọn _e*d_ sao cho chia $\phi(n)$ dư 1, hay $d= e^{-1} \pm \phi(n)$, khi đó ta sẽ có điều cần thiết:

$X^{ed}$ = $X^{k \phi(n) +1 }$ = $(X^{\phi(n)})^d$ * X= 1*X = X

$\phi(n)$ có thể tính được khi đã biết công thức phân tích thừa số nguyên tố của n, cụ thể là nếu đã biết n = p*q (p.q là số nguyên tố) thì $\phi(n)=(p-1)(q-1)$
Nói cách khác nếu như cho trước một số e thì nếu đã biết công thức phân tích thừa số nguyên tố của n ta có thể dễ dàng tìm được d sao cho $d=e^{-1}\pm\phi(n)$ hay là $X^{ed}=X\pm n$ , còn nếu không biết thì rất khó

# Mô tả thuật toán 
RSA sử dụng biểu thức mũ. Bản rõ được mã hoá bằng các khối, mỗi khối có một giá trị nhị phân nhỏ hơn 1 số _n_ nào đó. Do đó, kích thước mỗi khối phải nhỏ hơn hoặc bằng $log_2(n+1)$; trong thực tế, kích thước khối là i bit, trong đó $2^i<n\le2^{i+1}$. Mã hoá và giải mã có dạng sau, với khối bản rõ M và khối bản mã C.
> C = $M^e$ mod n
> M = $C^d$ mod n = $(M^e)^d$ mod n = $M^{ed}$ mod n

Cả bên nhận và bên gửi đều cần biết giá trị của _n_. Bên gửi biết giá trị của _e_, và chỉ có bên nhận biết giá trị của _d_. Do đó, đây là thuật toán PKC với khoá công khai _PU_ = {e,n} và khoá bị mật _PR_ = {d,n}. Để thuật toán thoả này thoả mãn PKC cần các điều kiện sau:
1. Có thể tìm được _e,d,n_ sao cho $M^{ed}$ mod n = M với mọi M < n
2. Dễ dàng tính được $M^e$ mod n và $C^d$ mod n với mọi M < n
3. Không thể xác định _d_ khi có _e_ và _n_

Xét điều kiện đầu tiên, ta cần tìm : $M^{ed}$ mod n = M.  Ta thấy e và d phải là nghich đảo nhân theo modulo $\phi(n)$, trong đó $\phi(n)$ là hàm phi Euler. Ta đã biết với p, q là số nguyên tố, $\phi(pq)$ = (p-1)(q-1). Quan hệ giữa e và d là: 
> ed mod $\phi(n)$ = 1

Ta cũng có thể nói: 
>  $ed \equiv 1$ mod $\phi(n)$
>  $d \equiv e^{-1}$ mod $\phi(n)$

Do đó, e và d là nhân nghịch đảo mod $\phi(n)$. Điều này chỉ đúng nếu gcd($\phi(n)$,d)=1

Figure 1: Tóm tắt thuật toán RSA

| Key Generation Alice   |    |
| -- | -- |
| Chọn p, q | p, q đều là số nguyên tố, $p \neq q$ |
| Tính n = $p \times q$ | |
| Tính $\phi(n) = (p-1)(q-1)$ | |
| Chọn số nguyên e | gcd($\phi(n)$,e) = 1; 1 < e < $\phi(n)$ |
| Tính d |$d \equiv e^{-1}$(mod $\phi(n)$) |
| Khoá công khai |PU = {e,n} |
| Khoá bí mật | PR = {d,n}|

| Encryption by Bob with Alice’s Public Key    |     |
| -- | -- |
| Bản rõ | M < n |
| Bản mã | C = $M^e$ mod n |

| Decryption by Alice with Alice’s Public Key  | |
| -- | -- |
| Bản mã | C |
| Bản rõ | M = $C^d$ mod n |

Giả sử M là khối tin gốc, C là khối mã tương ứng của M và ($z_A$,$Z_A$)  là  các thành phần công khai và riêng của khoá của Alice  
**Mã hoá** Nếu Bob muốn gửi một thông báo mã hoá cho Alice thì anh ta chỉ việc dùng khoá  công khai của Alice để thực hiện: 
> C = $E_{Z_A}$(M) = $M^e \pm n$

**Giải mã** Khi Alice muốn giải mã Y, cô ta chỉ việc dùng khoá riêng $z_A$ = d để thực hiện như sau:
> $D_{z_A}(C)$ = $C^d \pm n$

Để tiện cho việc giao dịch trên mạng có sử dụng truyền tin mật, người ta có thể thành lập các Public Directory (thư mục khoá công khai), lưu trữ các khoá công khai của các user.  
Thư mục này được đặt tại một điểm công cộng trên mạng sao cho ai cũng có thể truy nhập tới được để lấy khoá công khai của người cần liên lạc.

| User |(n,e) |
| -- | -- |
|Alice | (85,23) |
|Bob |(117,5) |
|Cathy |(4757,11) |
|.. |.. |


**Một số vấn đề xung quanh thuật toán RSA**
**Vấn đề chọn p và q**
 + p và q phải là những số nguyên tố lớn, ít nhất là cỡ 100 chữ số
 + p và q phải lớn cỡ xấp xỉ nhau ( về độ dài cùng 100 chữ số chẳng hạn)

**Một vài con số về tốc độ thuật toán trong cài đặt:**
So sánh với DES thì RSA:  
+ Có tốc độ chậm hơn rất nhiều. Thường thì, RSA chậm ít nhất là 100 lần khi cài đặt bằng phần mềm, và có thể chậm hơn từ 1000 đến 10,000 lần khi cài đặt bằng phần cứng (còn tùy cách cài đặt)  
+ Kích thước của khoá mật lớn hơn rất nhiều. Nếu như p và q cần biểu diễn cỡ 300 bits thì n cần 600 bits. Phép nâng lên luỹ thừa là khá chậm so với n lớn, đặc biệt là nếu sử dụng phần mềm (chương trình). Người ta thấy rằng thực hiện một phép nhân cỡ m + 7 nhịp Clock khi kích thước n là m bit.

**Về bài toán phân tích ra thừa số nguyên tố**
Giải thuật tốt nhất vẫn là phương pháp sàng số. Một ước lượng về thời gian thực hiện của giải thuật là:
> $L(n) \approx 10^{{9.7+ {\frac 1 {50}}} log_2n}$

Trong đó $log_2n$ cho số biết số bit cần để biểu diễn n, số cần phân tích ra thừa số nguyên tố.  Từ đó rút ra, nếu tăng n lên thêm 50 bit (quãng 15 chữ số thập phân) thì thời gian làm phân  tích ra thừa số nguyên tố tăng lên 10 lần.
Vào những năm cuối của thế kỷ 20, người ta đã ước lượng thấy, với n=200, $L(n) \approx 55$ ngàn năm. Đối với khả năng thực hiện bằng xử lý song song, một trong các kết quả tốt nhất về phân tích TSNT với số lớn cho biết đã phân tích một số có 129 chữ số, phân bố tính toán trên toàn mạng Internet và mất trọn 3 tháng

**Vấn đề đi tìm số nguyên tố lớn:**
Một thuật toán để tạo ra tất cả các số nguyên tố là không tồn tại, tuy nhiên có những thuật toán khá hiệu quả để kiểm tra xem một số cho trước có phải là nguyên tố hay không (bài toán kiểm tra tính nguyên tố). Thực tế, việc tìm các số nguyên tố lớn cho RSA là một vòng lặp như sau:  
1. Chọn một số ngẫu nhiên p nằm trong một khoảng có độ lớn yêu cầu (tính theo bit)  
2. Kiểm tra tính nguyên tố của p, nếu là nguyên tố thì dừng lại, nếu không thì quay lại bước 1.  

Những thuật toán tất định để kiểm tra tính nguyên tố là khá tốn thời gian và đòi hỏi được thực hiện trên máy tính có tốc độ cao. Tuy nhiên người ta cũng còn sử dụng các thuật toán xác suất, có khả năng ‘đoán’ rất nhanh xem một số có phải nguyên tố không. Các thuật toán xác suất này không đưa ra quyết định đúng tuyệt đối, nhưng cũng gần như tuyệt đối; tức là xác suất báo sai có thể làm nhỏ tùy ý, chỉ phụ thuộc vào thời gian bỏ ra.  
Xét ví dụ một thuật toán xác suất, dựa trên phương pháp sau đây của Lehmann.  
*Phương pháp Lehmann:* Giả sử n là một số lẻ, với mỗi số nguyên a ta hãy ký hiệu:
> G(a,n) = $a^{\frac {n-1}2} \pm n$
Ví dụ: Với n=7, ta có $2^3$=1, $3^3$=6, $4^3$=1, $5^3$=6, $6^3$=1; tức là G= {1,6}\

Theo Lehmann, nếu n là một số lẻ thì G(a,n)={1,n-1} với mọi a nguyên khi và chỉ khi n là số nguyên tố. Tuy nhiên với n hợp số, khả năng G(a,n)={1,n-1} vẫn xảy ra với xác suất 50% cho mỗi số nguyên a nguyên tố cùng nhau với n lựa chọn bất kỳ. Từ kết quả này, ta có phép thử như sau khi cần xác định tính nguyên tố của một số nguyên n:
1. Chọn ngẫu nhiên một số $a \in {Z_n}^*$
2. If (gcd(a,n) >1) return (“là hợp số”) else if
	1. If ($a^{\frac {n-1}2} = 1 \parallel a^{\frac {n-1}2} = -1$)) return("có thể là số nguyên tố") 
	2. else return("là hợp số")

Nếu như thực hiện phép thử này 100 lần và luôn thu được câu trả lời “có thể là nguyên tố” thì xác suất n không phải là số nguyên tố (‘đoán nhầm’) sẽ chỉ là $2^{-100}$  
Để có thể tìm được số lớn với tính nguyên tố chắc chắn tuyệt đối, người ta có thể sử dụng phương pháp xác suất này để loại bỏ nhanh chóng các hợp số và chỉ thực hiện phép kiểm tra tất định cuối cùng với các số đã đáp ứng tốt ở phép thử.

**Giải thuật tính luỹ thừa nhanh**
Luỹ thừa có thể được tính như thông thường bằng phép nhân liên tục tuy nhiên tốc độ sẽ chậm. Luỹ thừa trong trường $Z_n$ (modulo n) có thể tính nhanh hơn nhiều bằng giải thuật sau đây. Giải thuật này sử dụng hai phép tính là tính bình phương và nhân.

Để tính $M^{\alpha}$(modul n):
1. Xác định các hệ số $\alpha _i$ trong khai triển của $\alpha$ trong hệ nhị phân:
> $\alpha$ = $\alpha_0 2^0$ + $\alpha_1 2^1$ + + $\alpha_2 2^2$ + ... + $\alpha_k 2^k$
2. Dùng vòng lặp k bước để tính k giá trị $M^{2^i} \pm n$ , với i=1,k :
> $M^2 = M \times M$
> $M^4 = M^2 \times M^2$
> ...
> $M^{2^k} = M^{2^{k-1}} \times M^{2^{k-1}}$
3. Từ bước 1, ta tính được $M^{\alpha} \pm n$ bằng cách đem nhân với nhau các giá trị $X^{2^i} \pm n$ đã tính ở bước 2 nếu như $\alpha_i$ tương ứng của nó là 1:
$$(M^{2^i})^{\alpha_i}=
\begin{cases}
1  & \text{,$\alpha_i$=0} \\
X^{2^i} & \text{,$\alpha_i$=1}
\end{cases}$$

**Điểm yếu của giải thuật RSA**
Trong hệ RSA, không phải tất cả các thông tin đều được che giấu tốt, tức là mọi khoá đều tốt và đều làm bản rõ thay đổi hoàn toàn
Ví dụ:

|   |   |
|---|---|
|n = 35 = 5 x 7,  <br>e = 5  <br>M = 8  <br>C = $M^e \pm 35$ = 8 = X!|m = 4 x 6  <br>(GCD (5,24) = 1)|

Đối với bất kỳ khoá nào tồn tại ít nhất 9 bản rõ bị ‘phơi mặt’, tuy nhiên đối với $n \ge 200$ điều đó không còn quan trọng. Mặc dù vậy phải chú ý là nếu e không được chọn cẩn thận thì có thể gần đến 50% bản rõ bị lộ.

Ví dụ 3.8: Với n = 35, e = 17  
{1, 6, 7, 8, 13, 14, 15, 20, 21, 27, 28, 29, 34} không che được

Người ta cho rằng có thể tránh được tình huống này nếu số nguyên tố được chọn là AN TOÀN. Một số nguyên tố được gọi là AN TOÀN nếu p=2p’+1 trong đó p’ cũng là số  nguyên tố.

**Đánh giá về an toàn của thuật toán RSA**
Sự an toàn của thành phần khoá mật (private key) phụ thuộc vào tính khó của việc PTTSNT các số lớn.  
Ký hiệu Z= (e,n) là khoá công khai.  
Nếu biết PTTSNT của n là n=$p\times q$ thì sẽ tính được m=$\phi(n)$ =(p-1)(q-1). Do đó tính được  $d=e^{-1}$ (mod m) theo thuật toán GCD mở rộng.  
Tuy nhiên nếu không biết trước p,q thì như đã biết không có một thuật toán hiệu quả nào  
để PTTSNT đối với n, tức là tìm được p,q, khi n lớn. Nghĩa là không thể tìm được m và do đó không tính được d

**Chú ý**: Độ an toàn của RSA chưa chắc hoàn toàn tương đương với tính khó của bài toán PTTSNT, tức là có thể tồn tại phép tấn công phá vỡ được RSA mà không cần phải biết PTTSNT của n, chẳng hạn nếu như có kẻ thành công trong các dạng tấn công sau:  
1. Đi tìm thành phần khóa mật  
Kẻ thù biết M và C với C=$D_z(M)$. Để tìm d nó phải giải phương trình:  
$$M = C^d \pm n  $$
Hay là tính d = $log_CM$
2. Đi tìm bản rõ:  
Kẻ thù biết C và e, để tìm được bản rõ M nó phải tìm cách tính căn thức bậc e theo đồng dư, để giải phương trình  
$$Y=X^e$$
