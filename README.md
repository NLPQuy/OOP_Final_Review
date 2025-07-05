# 📚 Lập trình Hướng đối tượng C++

---

## 📑 Mục lục

- [Chương 2: Constructor và Destructor](#chương-2-constructor-và-destructor)
- [Chương 3: Operator](#chương-3-operator)
- [Chương 4: Xử lý Tập tin](#chương-4-xử-lý-tập-tin)
- [Chương 5: Kế thừa](#chương-5-kế-thừa)
- [Chương 6: Đa hình](#chương-6-đa-hình)
- [Chương 7: Xử lý Ngoại lệ](#chương-7-xử-lý-ngoại-lệ)
- [Chương 8: Template và Design Pattern](#chương-8-template-và-design-pattern)

---

## 🔧 Chương 2: Constructor và Destructor

### 🏗️ Phương thức tạo lập (Constructor)

#### **Đặc điểm của phương thức tạo lập:**
- Có tên trùng với tên lớp, không có kiểu trả về
- Tự động chạy khi đối tượng được khởi tạo, khởi tạo các dữ liệu và các công việc cần thiết để bắt đầu chu kỳ sống của đối tượng
- Có thể có nhiều phương thức tạo lập chồng nhau (overloading), chúng được phân biệt theo danh sách các tham số truyền vào

#### **Có 3 loại phương thức khởi tạo:**

**1. Phương thức khởi tạo mặc định (default constructor):**
- Phương thức tạo lập không có tham số đầu vào
- Được tạo mặc định nếu không định nghĩa phương thức khởi tạo

**2. Phương thức khởi tạo sao chép (copy constructor):**
- Được dùng để tạo ra một đối tượng từ một đối tượng sẵn có
- Trình biên dịch cung cấp sẵn phương thức tạo lập sao chép mặc định (default constructor), giúp tự động sinh ra lớp và giúp sao chép từng thành phần của thuộc tính (member-wise copy)
- Quá trình sao chép trên là sao chép theo dạng từng bit (bitwise copy), do đó nếu các thành phần dữ liệu là kiểu con trỏ thì quá trình sao chép sẽ chỉ sao chép địa chỉ con trỏ chứ không thật sự sao chép vùng mà chúng quản lý

**3. Phương thức khởi tạo nhận tham số đầu vào (parameter constructor):**
- Do người dùng định nghĩa với các tham số đầu vào cho đối tượng

#### **Ví dụ:**

```cpp
class fraction {
    private:
        int numerator, denominator;
    public:
        // Phương thức khởi tạo mặc định
        fraction(){}; 
        
        // Phương thức khởi tạo sao chép
        fraction(const fraction& p){
            numerator = p.numerator;
            denominator = p.denominator;
        }
        
        // Phương thức khởi tạo tham số 
        fraction(int _numerator, int _denominator){
            numerator = _numerator;
            denominator = _denominator;
        }
}

// Sử dụng
fraction p; // Phương thức khởi tạo mặc định
fraction q = fraction(p) // Phương thức khởi tạo sao chép
fraction r = fraction(2, 3) // Phương thức khởi tạo tham số
```

### 🗑️ Phương thức hủy (Destructor)

#### **Lý do:**
Khi đối tượng không còn được sử dụng nữa thì chúng sẽ trở thành rác, nếu không hủy sẽ gây tiêu tốn tài nguyên cấp phát, dẫn đến tràn RAM → Cần phương thức hủy đảm bảo nhiệm vụ này.

#### **Đặc điểm của phương thức hủy:**
- Được đặt tên trùng với tên lớp và có dấu ~ đặt ở ngay phía trước
- Không có tham số đầu vào, không có giá trị trả về
- Mỗi lớp chỉ có duy nhất một phương thức hủy
- Phương thức hủy tự động chạy mỗi khi đối tượng của lớp bị hủy (hết phạm vi sử dụng)
- Trong quá trình sống của đối tượng có và chỉ có duy nhất 1 lần phương thức hủy được gọi

#### **Ví dụ:**

```cpp
class mylist{
    private: 
        int* a = new int[100];
    public:
        // Phương thức hủy
        virtual ~mylist(){
            delete [] a; 
        }
}

// Sử dụng phương thức hủy:
mylist *arr;
delete arr;
```

---

## ⚡ Chương 3: Operator

### 🎯 Toán tử

- C++ cho phép định nghĩa việc định nghĩa toán tử trên các lớp để thực hiện các biểu thức tính toán tự nhiên
- Khi sử dụng đối tượng với toán tử định nghĩa thì hàm toán tử sẽ được gọi
- Chỉ có thể định nghĩa một số toán tử cho trước trong danh mục, không thể định nghĩa các toán tử mới
- Đối với từng toán tử, có thể nạp chồng với nhiều tham số khác nhau

### 📝 Toán tử gán

**Toán tử gán (operator =)** được sử dụng để gán giá trị của một biến này cho một biến khác.

C++ tự động cung cấp toán tử gán mặc định member-wise copy. Do vậy, đối với trường hợp thuộc tính là con trỏ, toán tự gán chỉ copy địa chỉ con trỏ từ đối tượng nguồn sang đối tượng đích chứ không thực sự sao chép nội dung vùng nhớ → Ta cần định nghĩa lại toán tử gán

**Cú pháp toán tử gán:**
```cpp
TenLopDT& operator = (const& TenLopDT& src)
```

**Ví dụ đối với class fraction:**
```cpp
fraction :: fraction& operator=(const fraction& src){
    if (this != &src) { 
        // Điều kiện if giúp tránh trường hợp gán bằng chính nó
        this->numerator = src.numerator;
        this->denominator = src.denominator;
        return *this;
    }
}
```

### 🔢 Các toán tử khác

#### **Toán tử +, -, *, /, >=, <=, > , <, ==:**

```cpp
fraction :: fraction& operator + (const fraction& other){
    fraction res;
    res.numerator = nomiator*other.denominator + other.numerator*denominator;
    res.denominator = other.denominator*denominator;
    return *res;
}
```

#### **Toán tử ++, +=, và các loại tương tự (tiền tố và hậu tố):**

```cpp
// Tiền tố cho ++
fraction :: fraction&  ++ operator (){
    numerator += denominator;
    reduce(); // nếu có định nghĩa thì dùng
    return *this;
}

//Toán tử hậu tố ++
fraction :: fraction operator ++(){
    fraction temp = *this; //Lưu lại giá trị hiện tại
    numerator += denominator;
    reduce() //nếu có định nghĩa thì dùng
    return temp;
}

// Toán tử +=
fraction :: fraction& operator += (const fraction& other){
    numerator = numerator*other.denominator + other.numerator*denominator;
    denominator = denominator*other.denominator;
    reduce();
    return *this;
}
```

### 🔄 Toán tử ép kiểu

```cpp
fraction :: operator int() const {
    return numerator / denominator;
}
```

### 👥 Friend và ứng dụng

**Sử dụng cho các toán tử nhập/ xuất (>> và <<)**

**Cú pháp:**
```cpp
istream& friend operator >>(istream& is, fraction& p);
ostream& friend operator << (ostream& os, fraction& p);
```

---

## 📁 Chương 4: Xử lý Tập tin

### 🏗️ Các lớp cơ bản của hệ thống nhập xuất chuẩn C++

| Lớp | Mô tả |
|-----|-------|
| **ios** | Hỗ trợ chung cho các lớp nhập xuất (không phân biệt nhập hay xuất) |
| **istream** | Kế thừa trực tiếp từ lớp ios, hỗ trợ nhập hay đọc dữ liệu từ đối tượng nhập liệu |
| **ostream** | Kế thừa trực tiếp từ ios, hỗ trợ xuất hay ghi dữ liệu lên thiết bị xuất |
| **iostream** | Kế thừa trực tiếp từ istream và ostream, hỗ trợ cho cả hai thao tác nhập và xuất (hay đọc và ghi) dữ liệu |
| **ifstream** | Kế thừa trực tiếp từ istream, hỗ trợ đọc dữ liệu từ tập tin |
| **ofstream** | Kế thừa trực tiếp từ ostream, hỗ trợ ghi dữ liệu lên tập tin |
| **fstream** | Kế thừa trực tiếp từ iostream, hỗ trợ đọc và ghi dữ liệu lên tập tin |
| **istringstream** | Kế thừa trực tiếp tiếp từ istream, hỗ trợ việc đọc hay lấy dữ liệu từ đối tượng chuỗi nhập có kiểu istringstream |
| **ostringstream** | Kế thừa trực tiếp từ ostream, hỗ trợ việc ghi hay xuất dữ liệu lên đối tượng chuỗi có kiểu ostringstream |
| **stringstream** | Kế thừa trực tiếp từ iostream, hỗ trợ các thao tác lên chuỗi ký tự nhập và xuất |

### 🔧 Các thao tác trên file

#### **Mở file:**
- `ios::in` (đọc)
- `ios::out` (ghi)
- `ios::binary` (file nhị phân)
- `ios::ate` (đưa con trỏ file về cuối file)
- `ios::app` (chế độ append, đưa con trỏ về cuối file)
- `ios::trunc` (nếu ghi sẽ overwrite nội dung cũ)

#### **Đóng file:** 
```cpp
<Tên đối tượng file>.close()
```

#### **Ví dụ (nên sử dụng trong bài thi vì dễ dàng và ít lỗi):**

```cpp
#include <fstream>
fstream file ("input.txt", ios::in | ios::out);
if(!file.is_open()) {
    cerr << "Không thể mở file"<<endl;
    return 0;
}
string line;
// Đọc từng dòng trong file
while(getline(file, line)) {
    // Đọc từng từ trong file
    string word;
    istringstream ss (line);
    while(ss >> word) {
        cout << word << " ";
    }
    cout << endl;
}
file.close()
return 0;
```

### 🧭 Đọc vị trí con trỏ file

**seekg/ tellg:** dùng cho đọc file  
**seekp/ tellp:** dùng cho ghi file

#### **Cách dùng tellg và tellp:**

**tellg:** trả về vị trí hiện tại của con trỏ đọc
```cpp
fstream file ("input.txt");
if (!file.is_open()) {
    cerr << "Không thể đọc file"<<endl;
    return 0;
}
char ch;
file >> ch;
// Lấy vị trí của con trỏ đọc
streampos posRead = file.tellg();
cout << "Vị trí hiện tại của con trỏ đọc" << posRead << endl; //output:1
```

**tellp:** trả về vị trí hiện tại của con trỏ ghi
```cpp
// Với đoạn code trên, nếu ta lấy vị trí của con trỏ ghi
streampos posWrite = file.tellp()
cout << "Vị trí hiện tại của con trỏ ghi" << posWrite << endl; //output:0
```

#### **Cách dùng seekg(streampos position) và seekp(streampos position):**

**seekg(streampos position):** gán vị trí con trỏ đọc
```cpp
file.seekg(5): // đặt con trỏ đọc đến vị trí số 5 tính từ đầu file
file.seekg(5, ios:cur) // đặt con trỏ đọc đến vị trí số 5 tính từ vị trí hiện tại
file.seekg(-2, ios::end) // đặt con trỏ đọc đến vị trí số 2 đếm từ cuối file lên
```

**seekp(streampos position):** gán vị trí con trỏ ghi, cách dùng tương tự seekg

---

## 🧬 Chương 5: Kế thừa (Inheritance)

### 🎯 Khái niệm kế thừa và ví dụ

**Kế thừa trong lập trình hướng đối tượng** là việc định nghĩa một lớp mới dựa trên một lớp có sẵn → Lớp mới có những thuộc tính và phương thức của lớp có sẵn.

#### **Khi lớp B kế thừa lớp A, thì:**
- **Lớp A** là lớp cơ sở (base class), hoặc còn gọi là lớp cha (super class)
- **Lớp B** là lớp kế thừa, lớp con (sub class), hay còn gọi là lớp dẫn xuất (derived class)
- Lớp B có thể sử dụng lại các thuộc tính và phương thức của lớp A
- Lớp B có thể được bổ sung thêm một số thuộc tính và phương thức riêng

**Ví dụ:** Sau khi khai báo và định nghĩa lớp Animal, với các thuộc tính như hành động ăn, hành động tấn công, … ta có thể khai báo và định nghĩa lớp Dog là lớp con của Animal, có thêm các thuộc tính như sủa, tên chủ, …

#### **Khai báo kế thừa trong C++:**
```cpp
class <Lớp kế thừa> : <Loại kế thừa> <Lớp cơ sở>
```

**Ví dụ:**
```cpp
class Dog: public Animal
{
    private: 
        string ownerName, bool tiem_dai;
    public:
        void Sua();
        void VayDuoi();
}
```

### 🔐 Phạm vi kế thừa

Ta có thể chỉ ra phạm vi kế thừa bằng `<Loại kế thừa>` trong cú pháp kế thừa, cụ thể như sau:

| **QUYỀN TRUY XUẤT LỚP CHA** | **KẾ THỪA PUBLIC** | **KẾ THỪA PROTECTED** | **KẾ THỪA PRIVATE** |
|----------------------------|--------------------|-----------------------|---------------------|
| **PUBLIC** | Có thể truy xuất từ bên ngoài lớp cha và cũng là public trong lớp con | Có thể truy xuất từ bên ngoài lớp cha và trở thành protected trong lớp con | Có thể truy xuất từ bên ngoài lớp cha và trở thành private trong lớp con |
| **PROTECTED** | Dùng trong phạm vi lớp cha và trở thành protected trong lớp con | Dùng trong phạm vi lớp cha và trở thành protected trong lớp con | Dùng trong phạm vi lớp cha và trở thành private trong lớp con |
| **PRIVATE** | Chỉ dùng trong phạm vi lớp cha, không thể truy xuất từ bên ngoài hay lớp con | Chỉ dùng trong phạm vi lớp cha, không thể truy xuất từ bên ngoài hay lớp con | Chỉ dùng trong phạm vi lớp cha, không thể truy xuất từ bên ngoài hay lớp con |

### 🏗️ Sử dụng constructor

Constructor không được kế thừa, phải viết lại constructor của lớp con trong đó có thể gọi constructor của lớp cha.

#### **Gọi constructor của lớp cha:**

**Default:** constructor của lớp con sẽ ngầm định gọi default constructor của lớp cha

**Để tường minh gọi constructor của lớp cha thì sử dụng cú pháp:**
```cpp
LopCon : LopCon(kieudulieux, kieudulieu y) : LopCha(x, y)
```

**Ví dụ:**
```cpp
Square :: Square(float w) : Rectangle (w, w){
}
```

**Trình tự khởi tạo đối tượng:** Constructor của lớp cha sẽ được gọi trước, sau đó mới tới constructor của lớp con.

### 🗑️ Sử dụng destructor

**Destructor của lớp con sẽ được gọi trước, sau đó mới tới destructor của lớp cha**

### 🔄 Sử dụng con trỏ lớp cha

Lớp B kế thừa lớp A, ta có thể sử dụng như sau:

```cpp
A* objA = new B();
```

hoặc:

```cpp
B objB;
A* p = &objB;
```

### 🌳 Các loại kế thừa

Xem trong link: https://github.com/NLPQuy/Inheritance-/blob/main/README.md

---

## 🎭 Chương 6: Đa hình (Polymorphism)

### 🌟 Khái niệm đa hình

**Đa hình (Polymorphism)** là khả năng của các đối tượng thuộc các lớp khác nhau có thể được xử lý thông qua cùng một interface (giao diện), trong đó hành vi cụ thể được xác định tùy theo loại thực tế của đối tượng đó tại thời điểm chạy chương trình (runtime).

**Ví dụ:** Giả sử có hai class là Rectangle và Square, trong đó Square chính là lớp con của Rectangle. Để nhập một Rectangle vào chương trình, ta cần nhập vào chiều dài và chiều rộng của nó, nhưng đối với Square thì chỉ cần nhập độ dài một cạnh. Vì vậy, phương thức nhập của Square sẽ khác đôi chút so với phương thức nhập của Rectangle. Tuy nhiên, nếu ta muốn một đối tượng Rectangle nắm giữ một đối tượng Square, thì ta cũng muốn nếu gọi phương thức nhập thì nó bắt buộc phải sử dụng phương thức nhập của Square. Đây là lúc cơ chế đa hình cần thiết.

**VD:**
```cpp
Rectangle *pRec; //Khởi tạo con trỏ đến đối tượng thuộc lớp Rectangle
Rectangle Rec;
Square Sq;
pRec = &Sq;
pRec -> Input(cin) //Ngay lúc này ta muốn cơ chế đa hình hoạt động
```

### 🎮 Đa hình trong OOP C++

Để sử dụng đa hình, đối với các phương thức nào của lớp cha mà ta muốn custom lại tại lớp con, ta cần sử dụng từ khóa **virtual** (cho biết đây là phương thức ảo) trước phương thức đó.

#### **Phương thức thuần ảo:**
Là các phương thức chỉ có phần khai báo mà không có cài đặt, được khai báo bằng cách gán bằng 0

**VD:** `Input() = 0;`

#### **Lớp trừu tượng (Abstract class):**
Là lớp không có bất kỳ đối tượng hay thể hiện nào, tức là những lớp này không thể tạo ra đối tượng của chúng. Một lớp là lớp trừu tượng khi nó chứa ít nhất một phương thức thuần ảo.

#### **Interface (pure abstract class):**
Interface là một lớp chỉ chứa các hàm thuần ảo, không chứa thuộc tính, dùng để định nghĩa bộ khung mà các lớp con phải triển khai

### 🔄 Phân biệt Overloading và Overriding

| | **Overloading** | **Overriding** |
|---|---|---|
| **Định nghĩa** | Nhiều hàm cùng tên, khác tham số trong cùng 1 lớp | Lớp con định nghĩa lại hàm ảo của lớp cha |

### 🔄 Ép kiểu với đa hình

Trong C++, **dynamic_cast** là công cụ quan trọng để ép kiểu an toàn khi làm việc với đa hình (polymorphism) – đặc biệt khi muốn chuyển kiểu giữa con trỏ hoặc tham chiếu đến các lớp có quan hệ kế thừa.

**Cú pháp:** `dynamic_cast<target_type>(expression)`

**VD:**
```cpp
Base* b = ...; 
Derived* d = dynamic_cast<Derived*>(b);
```

Nếu Derived không là lớp con của Base thì d là con trỏ null.

---

## ⚠️ Chương 7: Xử lý Ngoại lệ (Exception Handling)

### 🔍 Try
Giống như một người giám sát, tìm kiếm những exception và ném nó về một nơi để xử lý

```cpp
try {
    // Các dòng lệnh có thể phát sinh lỗi
}
```

### 🚀 Throw
Dùng để cho người dùng ném một ngoại lệ, người dùng có thể throw một câu thông báo hoặc 1 biến bất kỳ. Mỗi "throw" phải có ít nhất một catch. Nên throw ra bên ngoài các đối tượng thay vì các kiểu sẵn có. Trong C++, chúng ta tốt nhất nên throw các đối tượng thuộc lớp kế thừa lớp `std::exception`

**Cú pháp:** `throw the_error`

### 🎯 Catch
Khi ngoại lệ bị ném đi thì đây là nơi bắt ngoại lệ để xử lý thông báo.

**Cú pháp:**
```cpp
try {
    // Các dòng lệnh có thể phát sinh lỗi
}
catch (data_type_1 variable_name_1){
    // xử lý lỗi cho data_type_1
}
catch (data_type_2 variable_name_2){
    // xử lý lỗi cho data_type_2
}
```

### 📋 Lưu ý quan trọng

- Có thể catch biến theo kiểu giá trị, tham chiếu hay con trỏ. Thông thường nên catch biến theo kiểu tham chiếu (`catch(exception&e)`)
- Nếu lệnh throw không có giá trị trả về được gọi trong khối lệnh try{} thì phần catch sẽ không được xử lý. Thay vào đó chương trình sẽ kết thúc ngay lập tức
- Phải có ít nhất một khối lệnh catch theo liền sau khối lệnh try
- Khối lệnh catch chỉ được xem xét thực hiện nếu trong try{} có xảy ra lệnh throw

### 💡 Ví dụ

```cpp
#include <iostream>
#include <exception>
using namespace std;

int main() {
    try {
        throw exception();  // ném ra một ngoại lệ chuẩn
    }
    catch (exception& e) {  // bắt ngoại lệ theo tham chiếu
        cout << "Loi: " << e.what() << endl;
    }
    return 0;
}
```

---

## 🛠️ Chương 8: Khuôn mẫu (Template) và Mẫu thiết kế (Design Pattern)

### 🎨 Khuôn mẫu

**Template (Khuôn mẫu)** là một từ khóa được sử dụng để tạo ra các phương thức các lớp với nhiều loại kiểu dữ liệu khác nhau mà không cần phải viết lại nhiều lần

#### **Các loại Template:**

- **Class Template:** Khuôn mẫu lớp cho phép tạo các lớp khuôn mẫu tổng quát, bao gói dữ liệu và xử lý dữ liệu vào cùng một nơi, sử dụng một cách tổng quát
- **Function Template:** Khuôn mẫu hàm cho phép định nghĩa các hàm/ phương thức tổng quát dùng cho các kiểu dữ liệu tùy ý
- **Other Template:** Template specialization (Khuôn mẫu chuyên môn hóa)

### 🏗️ Tham số hóa cho lớp (Class Template)

**Cú pháp:**
```cpp
template <class T> class {
    // Nội dung của lớp.
}
```

**Ví dụ:**
```cpp
template <class T> 
class MyStack {
private:
    struct Node {
        T* data;
        Node* Next;
    }
public:
    void push(T* x); 
    T* pop();
    int len();
}
```

### ⚙️ Tham số hóa cho hàm (Function Template)

**Cú pháp:**
```cpp
template <class T> 
<Kiểu trả về> <Tên hàm>(<Danh sách các tham số>){
    // Nội dung của hàm
}
```

**Ví dụ: Hoán đổi hai số**
```cpp
template <class T>
void swap(T& a, T& b){
    T temp = a;
    a = b;
    b = temp;
}
```

### 🏗️ Mẫu thiết kế (Design Patterns)

**Mẫu thiết kế (Design Patterns)** là một giải pháp tổng thể cho cho các vấn đề trong thiết kế phần mềm theo hướng đối tượng. Nó không phải là chuẩn được quy định của bất kỳ ai.

#### **Lợi ích:**
- **Tính uyển chuyển:** 1 khuôn mẫu dùng để giải quyết một vấn đề nào đó. Nó không phải thiết kế cuối cùng
- **Dễ nâng cấp:** Nâng cấp giao diện, tính năng phục vụ một công việc nào đó
- **Dễ thay đổi:** Thay đổi theo các yêu cầu của khách hàng

#### **Design Patterns gồm ba loại chính: Creational, Structural, và Behavioral**

### 🎯 Creational Patterns
Giúp khắc phục các vấn đề về khởi tạo đối tượng, hạn chế sự phụ thuộc nền tảng (gồm 5 mẫu là Factory, Abstract Factory, Singleton, Prototype, Builder).

**Điển hình nhất là mẫu Singleton.**

#### **Sử dụng Singleton khi:**
- Đảm bảo rằng chỉ có một thể hiện của đối tượng riêng biệt tại một thời điểm bất kì
- Có thể quản lý số lượng thể hiện của một lớp trong giới hạn chỉ định
- Người lập trình có thể kế thừa mà không cần thay đổi các đoạn mã của chương trình

### 🏗️ Structural Patterns
Liên quan đến các vấn đề làm thế nào để các lớp và đối tượng kết hợp với nhau tạo thành cấu trúc lớn hơn (gồm 7 mẫu là Adapter, Bridge, Composite, Decorator, Facade, Flyweight và Proxy)

**Mẫu điển hình là Composite, thường gặp ở các bài toán trong lớp có lớp lồng nhau.**

#### **Sử dụng Composite khi:**
- Tạo ra một cấu trúc giống như cấu trúc cây
- Tất cả các đối tượng trong cấu trúc được thao tác một cách thuần nhất như nhau

### 🎭 Behavioral Patterns
Mô tả hành vi của đối tượng, cách thức để các lớp hoặc đối tượng có thể giao tiếp với nhau (gồm 11 mẫu là Interpreter, Template Method, Chain of Responsibility, Command, Iterator, Mediator, Memento, Observer, State, Strategy, và Visitor)

**Điển hình nhất là mẫu State. Đây là mẫu thường gặp ở các dạng thiết kế game**

#### **Sử dụng State khi chúng ta muốn:**
- Hành vi của đối tượng có thể phụ thuộc vào trạng thái hiện hành của đối tượng đó. Cách tốt nhất để giúp đối tượng thay đổi linh động và dễ dàng 1 hành vi phù hợp theo từng trạng thái là dùng mẫu state

---

## 🎯 Tóm tắt

Tài liệu này cung cấp kiến thức toàn diện về **Lập trình Hướng đối tượng trong C++**, bao gồm các chủ đề từ cơ bản đến nâng cao:

- ✅ **Constructor & Destructor** - Quản lý vòng đời đối tượng
- ✅ **Operator Overloading** - Tùy chỉnh toán tử cho lớp
- ✅ **File I/O** - Xử lý tập tin và luồng dữ liệu  
- ✅ **Inheritance** - Kế thừa và tái sử dụng code
- ✅ **Polymorphism** - Đa hình và virtual functions
- ✅ **Exception Handling** - Xử lý ngoại lệ an toàn
- ✅ **Templates & Design Patterns** - Lập trình generic và mẫu thiết kế

Đây là tài liệu tham khảo hữu ích cho việc học tập và ôn thi môn Lập trình Hướng đối tượng C++.
