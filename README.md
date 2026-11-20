# 🏦 Bank Management System — C++ Console Application

![Language](https://img.shields.io/badge/language-C%2B%2B-blue)
![Platform](https://img.shields.io/badge/platform-Windows-lightgrey)
![Storage](https://img.shields.io/badge/storage-Text%20Files-green)
![Status](https://img.shields.io/badge/status-Educational-orange)

A console-based bank management system written in modern C++. It manages clients, deposits and withdrawals, and multiple system users with **fine-grained, bitwise permissions**. Data is persisted in plain text files, so no database is required.

The project is intentionally small and dependency-free, which makes it a good reference for:

- Structuring a procedural C++ program with `struct`, `enum`, and `vector`
- File I/O with `fstream` (read, overwrite, append)
- Parsing delimited records
- Implementing a **bitwise permission system**
- Building menu-driven console applications

---

## ✨ Features

**Client management**
- List all clients in a formatted table
- Add one or many clients (duplicate account numbers are rejected)
- Update client information
- Delete a client (with confirmation)
- Find a client by account number

**Transactions**
- Deposit
- Withdraw (validates that the amount does not exceed the balance)
- Total balances report

**User management and security**
- Login screen with username/password check
- Add, list, update, delete, and find users
- Per-user permissions (full access or any combination of 7 permissions)
- "Access Denied" screen when a user lacks permission
- A protected default admin account that cannot be deleted

---

## 🔐 Permission System

Permissions are stored as a single integer using bit flags.

| Value | Permission        |
|------:|-------------------|
| `-1`  | Full access (all) |
| `1`   | Show client list  |
| `2`   | Add new client    |
| `4`   | Delete client     |
| `8`   | Update client     |
| `16`  | Find client       |
| `32`  | Transactions      |
| `64`  | Manage users      |

A check is just a bitwise AND:

```cpp
bool CheckPerimissions(short BitWise)
{
    if (Permissoins == -1) return true;
    return (BitWise & Permissoins);
}
```

**Example:** a user who may list and find clients only has `1 + 16 = 17`.

---

## 📁 Project Structure

```
BankProject/
├── main.cpp        # All source code
├── Clients.txt     # Clients data (created automatically on first add)
├── Users.txt       # Users data (must contain at least one admin to log in)
└── README.md
```

### Data file formats

Records are stored one per line, separated by `#//#`.

**Clients.txt**
```
AccountNumber#//#PinCode#//#Name#//#Phone#//#AccountBalance
A150#//#1234#//#Mohammed Abu-Hadhoud#//#079999#//#5000.000000
```

**Users.txt**
```
UserName#//#Password#//#Permissions
Muhammad Sadaka#//#1234#//#-1
```

---

## 🚀 Getting Started

### Requirements
- **Windows** (the code uses `system("cls")` and `system("pause>0")`)
- A C++11 (or newer) compiler: Visual Studio 2019+, or MinGW g++

### 1. Clone
```bash
git clone https://github.com//.git
cd 
```

### 2. Create the first admin user
Create `Users.txt` next to the executable with one line:
```
Muhammad Sadaka#//#1234#//#-1
```
> The user named `Muhammad Sadaka` is hard-coded as undeletable. Change the name in `DeleteUserByName` if you want a different protected admin.

### 3. Build and run

**Visual Studio:** open the project and press `Ctrl + F5`.

**g++ (MinGW):**
```bash
g++ main.cpp -o bank.exe
bank.exe
```

### 4. Log in
Use the username and password from `Users.txt`.

---

## 🖥️ Usage

```
===========================================
                Main Menue Screen
===========================================
        [1] Show Client List.
        [2] Add New Client.
        [3] Delete Client.
        [4] Update Client Info.
        [5] Find Client.
        [6] Transactions.
        [7] Manage Users.
        [8] Logout.
===========================================
```

Add your own screenshots to a `docs/` folder and link them here:

```markdown
![Main menu](docs/main-menu.png)
```

---

## 🧱 How It Works

1. `Login()` loads `Users.txt`, validates credentials, and stores the user's permission value.
2. Each menu option calls `CheckPerimissions(...)` before running its screen.
3. Data flows through four steps: **file → `vector` → modify in memory → overwrite file**.
4. Deletion uses a `MarkForDelete` flag; marked records are skipped when the file is saved.

Key building blocks: `SplitString`, `ConvertLinetoRecord`, `ConvertRecordToLine`, `LoadCleintsDataFromFile`, `SaveCleintsDataToFile`, `AddDataLineToFile`.

---

## ⚠️ Known Limitations

This is an educational project. Please do not use it for real financial data.

- Passwords and PINs are stored in **plain text**
- Input is not validated (for example, entering letters where a number is expected)
- Windows only because of `system("cls")` / `system("pause")`
- Menu navigation uses recursion; long sessions grow the call stack
- Some functions do not return a value on every path
- No transaction history or audit log
- `Login()` has no exit option other than closing the window

## 🗺️ Roadmap

- [ ] Hash passwords (for example, SHA-256)
- [ ] Input validation and error handling
- [ ] Transaction history log
- [ ] Transfer between accounts
- [ ] Cross-platform screen clearing
- [ ] Split code into headers and source files
- [ ] Unit tests

---

## 🤝 Contributing

Contributions are welcome.

1. Fork the repository
2. Create a branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

Released under the [MIT License](LICENSE).

## 👤 Author

**Muhammad** — Junior Software Developer, Damascus, Syria

- GitHub: [@Muhammad-sadaka](https://github.com/Muhammad-sadaka)

If this project helped you learn, please give it a ⭐