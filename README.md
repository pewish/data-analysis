# data-analysis

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
