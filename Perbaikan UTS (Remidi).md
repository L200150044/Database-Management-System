# Ujian Tengah Semester Ganjil 2017/2018

- Nama : Abdul Kemal Nasa'i Wibowo
- NIM  : L200150043

## Soal :
1.	Lihat gambar-1, gambar tersebut menampilkan semua tabel yang terdapat dalam database ‘classicmodels’. (a) Buatlah semua relasi antar tabel, dan terangkan jenis relasinya (20%). (b) Jelaskan fungsi masing-masing tabel (digunakan untuk menyimpan data apa?) (10%). 
2.	Buatlah query untuk menampilkan daftar barang yang terjual pada tanggal tertentu! Kolom output minimal terdiri atas kode, nama barang, dan jumlah barang yang terjual. (20%)
3.	Sama dengan soal kedua tetapi dibuat dalam bentuk store procedure dengan tanggal sebagai input, sebagai output adalah nilai uang yang diperoleh di tanggal tersebut, dan tabel daftar barang. (30%)
4.	Buatlah store procedure untuk menampilkan daftar customer yang melakukan pembelian (order) selama satu minggu terakhir, sebagai output adalam jumlah pelanggan dan daftar pelanggan. (25%).

![Gambar-1](https://raw.githubusercontent.com/L200150043/Database-Management-System/master/img/UTS-Gambar-1.png)

Gambar-1


## Penyelesaian :
1. A. Buatlah semua relasi antar tabel, dan terangkan jenis relasinya (20%)

![Relasi ClassicModel](http://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-Sample-Database-Schema.png)

#### Keterangan :
-	employees menempati office:
	- tabel utama : employees
  	- tabel kedua : office
    - relationship : one-to-many(1:n)
    - attribute penghubung : employeeNumber,officeCode(FK officeCode di employees
- employees melayani costumer
  - tabel utama : employees
  - tabel kedua : costumers
  - relationship : one-to-many(1:n)
  - attribute penghubung : employeeNumber,costumerNumber(FK employeeNumber di costumers)
- costumers melakukan payment
  - tabel utama :costumers
  - tabel kedua : payment
  - relationship :  one-to-many(1:n)
  - attribute penghubung : costumerNumber,checkNumber(FK costumerNumber di payment)
- orders dilakukan costumers
  - tabel utama : orders
  - tabel kedua : costumers
  - relationship : one-to-many(1:n)
  - attribute penghubung : orderNumber,costumerNumber(FK oderNumber di costumers)
- orderdetails menerangkan order
  - tabel utama : orderdetails
  - tabel kedua : order
  - relationship : one-to-many(1:n)
  - attribute penghubung : orderNumber,productCode(FK orderNumber,FK productCode di oerderdetails)
- productilines menjelaskan product
  - tabel utama : productilines
  - tabel kedua : product
  - relationship : one-to-many (1:n)
  - attribute penghubung : productiline,productCode(FK productiline di product)
- product menerangkan orderdetails
  - tabel utama : product
  - tabel kedua : orderdetail
  - relationship : one-to-many (1:n)
  - attribute penghubung : productCode,orderNumber(FK productCode di orderdetails
  
  
B. Jelaskan fungsi masing-masing tabel (digunakan untuk menyimpan data apa?)
  - Customers : menyimpan data pelanggan
  - Employees : menyimpan data karyawan
  - Payments : meyimpan data transaksi
  - Productlines : menyimpan data detail products
  - Product : menyimpan data barang 
  - Offices : menyimpan data karyawan yang ada di kantor
  - Orderdetails : menyimpan detail pesanan 
  - Orders : meyimpan data pesanan


2. Buatlah query untuk menampilkan daftar barang yang terjual pada tanggal tertentu! Kolom output minimal terdiri atas kode, nama barang, dan jumlah barang yang terjual. (20%)

![SQL Command](https://raw.githubusercontent.com/L200150043/Database-Management-System/master/img/UTS-nomor-2.png)
  

#### SQL Query:

```sql
SELECT products.productcode, products.productname, orderdetails.quantityordered, orders.orderdate
FROM products, orders, orderdetails 
WHERE orders.orderdate='2003-01-10' AND products.productcode=orderdetails.productcode AND orderdetails.ordernumber=orders.ordernumber;
```

3. Sama dengan soal kedua tetapi dibuat dalam bentuk store procedure dengan tanggal sebagai input, sebagai output adalah nilai uang yang diperoleh di tanggal tersebut, dan tabel daftar barang. (30%)


![SQL Store Procedure](https://raw.githubusercontent.com/L200150043/Database-Management-System/master/img/RE-UTS-nomor-3.png)


#### SQL Query:
```sql
DELIMITER //
CREATE PROCEDURE getproductbydate(IN tanggal date, OUT uang int)
BEGIN
SELECT SUM(orderdetails.priceEach * orderdetails.quantityOrdered) INTO uang
FROM orders, orderdetails
WHERE orders.orderdate=tanggal AND orderdetails.ordernumber=orders.ordernumber;

SELECT products.productname, orderdetails.quantityordered, orderdetails.priceEach 
FROM products ,orders, orderdetails 
WHERE orders.orderdate=tanggal AND products.productcode=orderdetails.productcode AND orderdetails.ordernumber=orders.ordernumber;
END //
DELIMITER ;
```


4. Buatlah store procedure untuk menampilkan daftar customer yang melakukan pembelian (order) selama satu minggu terakhir, sebagai output adalam jumlah pelanggan dan daftar pelanggan. (25%)

![SQL Store Procedure](https://raw.githubusercontent.com/L200150043/Database-Management-System/master/img/UTS-nomor-4.png)


#### SQL Query:
```sql
DELIMITER //
CREATE PROCEDURE getcustomerbydate(IN tanggal date, OUT jumlah_cust int)
BEGIN
SELECT COUNT(customerNumber) INTO jumlah_cust
FROM orders
WHERE orderDate BETWEEN DATE_SUB(tanggal, INTERVAL 7 DAY) AND tanggal; 

SELECT customers.customernumber, customers.customername, orders.orderdate
FROM customers, orders
WHERE customers.customernumber=orders.customernumber AND orderDate BETWEEN DATE_SUB(tanggal, INTERVAL 7 DAY) AND tanggal;
END //
DELIMITER ;
```
