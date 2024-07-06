# Task 1: normal transaction with CRSF vulnerability

## 1.1: Login, Check balance
First we will launch the <span style="color:yellow">target.py</span> website. After launching we can see the website's interface:

<img width="726" alt="lab 3 1.1.1.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.1.1.png"><br>

We will change the path to access the login page and log in to the account of the user named Alice with <i>username</i> being <span style="color:red">alice</span> and <i>password</i> is <span style="color:red">alice</span>

<img width="726" alt="lab 3 1.1.2.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.1.2.png"><br>
<img width="726" alt="lab 3 1.1.3.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.1.3.png"><br>

Next, after successfully logging in, we continue to change the path to <span style="color:yellow">'/balance'</span> to check the account balance.

<img width="726" alt="lab 3 1.1.4.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.1.4.png"><br>

## 1.2: Doing the transaction
In this section, to transfer money to Bob's account, we will go to the transaction page via the link <span style="color:yellow">'/transfer'

<img width="726" alt="lab 3 1.2.1.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.2.1.png"><br>

After moving to the transaction page, enter the receiving account and the amount to transfer to perform the transaction.

<img width="726" alt="lab 3 1.2.2.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.2.2.png"><br>

After successful implementation, we return to check the balance again, then the balance will be deducted by the exact amount we just transferred:

<img width="726" alt="lab 3 1.2.3.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.2.3.png"><br>

## 1.3: Transfer money illegitimately
To perform a CSRF attack when user Alice accidentally downloads a file <span style="color:yellow">hidden_form.html</span>. We will launch the above website. As soon as the website is launched, user Alice will immediately lose 1000 and the attacker account will gain 1000:

<img width="726" alt="lab3_1.3.1.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.3.1.png"><br>

<img width="726" alt="lab3_1.3.2.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.3.2.png"><br>

Login into Attacker account, we can see this balance was increased
<img width="726" alt="lab3_1.3.3.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.3.3.png"><br>

<img width="726" alt="llab3_1.3.4.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_1.3.4.png"><br>

We can see that when launching the malicious html file, the input form to perform transactions of the official website was called to the fake website and at the same time it made transactions to an unwanted account, the account by Attacker. At that time, the attack through the CSRF vulnerability was performed.

# Task 2: CSRF Countermeasure implementation

## 2.1: Solution 1:
<span style="color:red"><b>Idea:</b></span> sử dụng CSRF Token, biện pháp này sẽ khởi tạo một giá trị ngẫu nhiên được tạo ra bởi máy chủ và được gửi đến client. Khi client gửi yêu cầu POST, token này sẽ được gửi kèm theo yêu cầu và máy chủ sẽ kiểm tra token để xác nhận yêu cầu hợp lệ.

<span style="color:red"><b>Step 1:</b></span> ta sẽ cài đặt thư viện Flask-WTF để sử dụng CSRF token cho trang web <span style="color:yellow">target.py</span> của chúng ta<br>

<img width="726" alt="lab3_2.1.1.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_2.1.1.png"><br>

<span style="color:red"><b>Step 2:</b></span> Cấu hình CSRF token trong trang web: <br>

<img width="726" alt="lab3_2.1.2.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_2.1.2.png"><br>

ta sẽ tiến hành cấu hình để kiểm tra token trước khi sử dụng phương thức POST <br>
<img width="726" alt="lab3_2.1.3.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_2.1.3.png"><br>

Sau khi đã cấu hình hoàn tất ta tiến hành chạy lại trang web đồng thời mở trang web độc hại dùng để tấn công lỗ hỏng trên ta sẽ thấy việc tấn công thất bại và đã bị chặn lại:

<img width="726" alt="lab3_2.1.4.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_2.1.4.png"><br>

<img width="726" alt="lab3_2.1.5.png" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/lab3_2.1.5.png"><br>

## 2.2: Solution 2: