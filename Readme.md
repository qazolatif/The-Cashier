# The Cashier
Aplikasi ini digunakan di kasir toko yang berguna untuk mencatat berbagai transaksi yang biasanya terjadi.
## Scope & Functionalities
* User dapat memasukkan nama item.
* User dapat memasukkan tipe item.
* User dapat memasukkan jumlah item.
* User dapat memasukkan harga item.
* User dapat melihat data-data item yang telah dimasukkan ke suatu kolom yang tersedia.
* User dapat mengetahui total harga dari semua item.
## How does it works?
Class `Item.cs` berfungsi untuk menyimpan variabel-variabel dan sebagai _getter_ dan _setter_ untuk variabel-variabel yang ada.
Contoh:
```C#
class Item
{
    private int id;
    public string title { get; set; }

    public Item(int id, string title, int quantity, string type, double price)
    {
        this.id = id;
        this.title = title;
    }

    public string getTitle()
    {
        return title;
    }
}
```

Class `Calculator.cs` berfungsi untuk membuat list yang diinputkan pengguna dan menjumlahkan total harganya.
Contoh:
```C#
class Calculator
    {
        private List<Item> listItem;
        private double total=0;

        public Calculator()
        {
            this.listItem = new List<Item>();
        }
        public void addItem(Item item)
        {
            this.listItem.Add(item);
            this.total += item.getSubTotal();
        }
        public double getTotal()
        {
            return total;
        }

        public List<Item> getListItem()
        {
            return listItem;
        }
    }
```

Pada aplikasi ini, terdapat suatu _code design_ yang dikenal dengan [SOLID](https://en.wikipedia.org/wiki/SOLID). 
Definisi ini dibuat oleh Robert C. Martin, contohnya adalah Single Responsibility. 
Apa itu Single Responsibility?

Salah satu hal yang ingin ditekankan dengan prinsip ini adalah agar rancangan yang dihasilkan tetap dapat memberikan faktor kohesi yang tinggi. 
Nah, pada Single Responsibility, kohesi dalam perangkat lunak merujuk pada pemahaman tentang seberapa dekat hubungan ketergantungan antara method (fungsi) dan atribut dalam sebuah class. 
Sebuah class akan memiliki kohesi tinggi jika menggunakan method-method dan variabel level class secara utuh untuk menyelesaikan suatu pekerjaan tertentu. 
Sebaliknya sebuah class dinilai memiliki kohesi yang rendah jika memiliki method-method yang disisipkan secara acak dan digunakan untuk memenuhi berbagai macam pekerjaan.

Contoh Single Responsibility dan penerapannya pada aplikasi ini adalah:
* Pada class `Item.cs`
```C#
class Item
{
    public string getTitle()
    {
        return title;
    }
    public int getQuantity()
    {
        return quantity;
    }
    public double getPrice()
    {
        return price;
    }
    public double getSubTotal()
    {
        subtotal = price * quantity;
        return subtotal;
    }
}
```
terlihat pada setiap class di `Item.cs` hanya memiliki 1 fungsi yang melekat pada class itu sendiri. Walaupun pada class **getSubTotal()** terdapat 2 fungsi, namun 2 fungsi tersebut merupakan gabungan fungsi yang menjadi satu yang bertanggung jawab pada class yang bersangkutan.
* Pada class `Calculator.cs`
```C#
class Calculator
{
    public Calculator()
    {
        this.listItem = new List<Item>();
    }
    public void addItem(Item item)
    {
        this.listItem.Add(item);
        this.total += item.getSubTotal();
    }
    public double getTotal()
    {
        return total;
    }
    public List<Item> getListItem()
    {
        return listItem;
    }
}
```
pada class `Calculator.cs`, masing-masing class yang ada memiliki 1 tanggung jawab berupa fungsi yang hanya melekat pada class yang bersangkutan. 
Contoh, class **getTotal()** hanya memiliki 1 fungsi tanggung jawab yaitu **return total**.