[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23574029&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** 26ai.giangvh@vinuni.edu.vn
**Name:** Vương Hoàng Giang

---

## Mo ta

Bai lab Day 10 xay dung mot ETL Pipeline tu dong va nghien cuu tac dong cua chat luong du lieu (Data Observability) len AI Agent.

Nhung gi da lam:
- **Extract**: Doc du lieu tu file `raw_data.json` gom 5 records san pham.
- **Validate**: Loai bo cac records khong hop le (gia <= 0 hoac category trong).
- **Transform**: Tinh `discounted_price` (giam 10%), chuan hoa `category` thanh Title Case, them cot `processed_at` (timestamp xu ly).
- **Load**: Luu ket qua hop le ra file `processed_data.csv`.
- **Agent Simulation**: Chay mo phong RAG-like agent voi du lieu sach vs du lieu "garbage" de so sanh chat luong ket qua.
- **Generate Garbage Data**: Tao file `garbage_data.csv` co cac loi pho bien (duplicate ID, sai kieu du lieu, outlier, null values) de stress-test agent.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Tao Garbage Data
```bash
python generate_garbage.py
```

### Chay Agent Simulation (Stress Test)
```bash
# Chay agent voi du lieu sach (processed_data.csv)
# va du lieu garbage (garbage_data.csv) de so sanh ket qua
python agent_simulation.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script (Extract, Validate, Transform, Load)
├── agent_simulation.py      # Mo phong RAG-like AI Agent
├── generate_garbage.py      # Script tao du lieu "ban" de thu nghiem
├── raw_data.json            # Du lieu nguon (5 records)
├── processed_data.csv       # Output cua pipeline (3 records hop le)
├── experiment_report.md     # Bao cao thi nghiem Clean vs Garbage data
└── README.md                # File nay
```

---

## Ket qua

- **Tong records trong raw_data.json**: 5
- **Records hop le (da xu ly)**: 3 (Laptop, Chair, Monitor)
- **Records bi loai**: 2
  - `Mystery Box` (id=3): bi loai vi `price = -10` (gia am)
  - `Phone` (id=4): bi loai vi `category` trong
- **Output**: `processed_data.csv` chua 3 records voi cac cot: `id`, `product`, `price`, `category`, `discounted_price`, `processed_at`

### Agent Simulation
| Scenario | Ket qua |
|---|---|
| Clean Data | Agent tra loi dung: "the best choice is Laptop at $1200" |
| Garbage Data | Agent bao loi vi file `garbage_data.csv` chua duoc tao truoc khi chay |
