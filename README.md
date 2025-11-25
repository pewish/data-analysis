# data-analysis

df_us_catalog_raw = pd.read_excel(
    US_CATALOG_PATH,
    sheet_name=US_CATALOG_SHEET,
    engine="pyxlsb"
)

print("Колонки catalog_US:")
print(df_us_catalog_raw.columns.tolist())
display(df_us_catalog_raw.head())

df_us_catalog = df_us_catalog_raw.copy()

# ищем по смыслу
def find_col(columns, *patterns):
    for c in columns:
        name = str(c).lower()
        if all(p in name for p in patterns):
            return c
    return None

col_category = find_col(df_us_catalog.columns, "category") \
    or find_col(df_us_catalog.columns, "категор")
col_driver   = find_col(df_us_catalog.columns, "driver") \
    or find_col(df_us_catalog.columns, "драйвер")

print("Определили колонки:")
print("  category <-", col_category)
print("  driver   <-", col_driver)

if col_category is None or col_driver is None:
    raise ValueError("Не удалось определить колонки category/driver в catalog_US. Посмотри вывод выше.")

df_us_catalog = df_us_catalog.rename(columns={
    col_category: "category",
    col_driver:   "driver_code"
})

df_us_catalog = df_us_catalog[["driver_code", "category"]].copy()

# Нормализация: str + strip + lower (чтобы не ловить отличия в регистре)
df_us_catalog["driver_code"] = (
    df_us_catalog["driver_code"]
    .astype(str)
    .str.strip()
    .str.lower()
)
df_us_catalog["category"] = df_us_catalog["category"].astype(str).str.strip()

df_us_catalog = df_us_catalog.drop_duplicates()

print("Каталог после подготовки:")
display(df_us_catalog.head())
print("Всего драйверов в каталоге:", df_us_catalog["driver_code"].nunique())
