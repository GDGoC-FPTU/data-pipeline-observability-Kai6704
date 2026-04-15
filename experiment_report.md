# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600349
**Name:** Vương Hoàng Giang
**Date:** 15/04/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is Laptop at $1200." | 9 | Tra loi chinh xac, du lieu da duoc validate va transform day du |
| Garbage Data (`garbage_data.csv`) | "Agent Error: I'm choking on the data! ([Errno 2] No such file or directory: 'garbage_data.csv')" | 1 | Agent bi loi hoan toan, khong the doc du lieu |

> **Ghi chu:** Sau khi chay `python generate_garbage.py` de tao file `garbage_data.csv`, agent doc duoc du lieu nhung tra ve ket qua sai lech do cac van de chat luong du lieu ben trong file (outlier, duplicate, sai kieu).

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

File `garbage_data.csv` duoc tao ra boi `generate_garbage.py` chua nhieu van de chat luong du lieu nghiem trong, cu the:

**1. Duplicate ID (id = 1 xuat hien 2 lan):**
Record `Laptop` va `Banana` cung co `id = 1`. Khi agent tim san pham tot nhat theo gia, logic co the bi nhap nho hoac tra ve ket qua khong nhat quan tuy thuoc vao thu tu xu ly. Du lieu trung lap gay kho khan cho viec xac dinh ban ghi nao la chinh xac.

**2. Sai kieu du lieu (price = 'ten dollars'):**
San pham `Broken Chair` co truong `price` la chuoi van ban thay vi so nguyen. Khi agent thuc hien phep so sanh `subset['price'].idxmax()`, Python se nem loi `TypeError` hoac tra ve ket qua vo nghia vi khong the so sanh chuoi voi so. Day la loi nghiem trong nhat vi no pha vo toan bo logic cua agent.

**3. Extreme Outlier (price = 999999):**
San pham `Nuclear Reactor` co gia 999,999 USD — gap gan 833 lan muc gia binh thuong. Neu agent chon san pham co gia cao nhat, no se tra loi "Nuclear Reactor" thay vi mot san pham dien tu co y nghia thuc te. Agent bi danh lua boi du lieu ngoai le, cho ra khuyen nghi vo ly trong thuc te.

**4. Null Values (id = None, price = 0, category = None):**
Record `Ghost Item` co `id` va `category` la `None`, `price = 0`. Khi agent loc theo category (`df['category'].str.lower()`), chuoi `None` se gay loi `AttributeError`. Cac gia tri null khong duoc xu ly truoc se lam crash agent o bat ky buoc filter nao.

Ket luan phan tich: Du lieu "garbage" tac dong theo nhieu huong khac nhau — mot so loi lam agent crash hoan toan, mot so loi khac khong gay loi ro rang nhung am tham dan den ket qua sai. Day chinh la su nguy hiem cua du lieu kem chat luong: khong phai luc nao cung nhin thay loi, nhung ket qua thi sai.

---

## 3. Ket luan

**Quality Data > Quality Prompt? — Dong y.**

Thong qua thi nghiem nay, co the thay ro rang du lieu sach (clean data) la nen tang bat buoc de AI Agent hoat dong chinh xac. Du prompt co duoc viet tot den dau, neu du lieu dau vao co loi (sai kieu, null, outlier, trung lap), agent se hoac la crash hoan toan, hoac tra ve ket qua sai lech ma nguoi dung khong the phat hien ngay.

ETL Pipeline trong `solution.py` da chung minh gia tri cua buoc Validate: tu 5 records dau vao, 2 records loi da bi loai bo, chi giu lai 3 records hop le. Nho do, agent chay voi `processed_data.csv` cho ket qua chinh xac va dang tin cay.

**Bai hoc:** Trong he thong AI thuc te, dau tu vao Data Quality (ETL, validation, observability) co hieu qua cao hon va ben vung hon so voi chi tap trung vao toi uu hoa prompt.
