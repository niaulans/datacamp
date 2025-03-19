## [Data Manipulation with pandas](https://app.datacamp.com/learn/courses/data-manipulation-with-pandas)

# %% [markdown]
# ## Transforming Data

# %% [markdown]
# - Rectangular data -> data tabel biasa atau biasa disebut tabular data
# - Pada pandas, tabular data direpresentasikan ke DataFrame object
# 
# - fungsi head() -> menampilkan beberapa data pertama DatFrame
# - fungsi info() -> menampilkan nama kolom, tipe data, value
# - fungsi shape() -> menampilkan jumlah baris dan kolom
# - fungsi describe() -> menampilkan statistik dari data (count, mean, edian, min, std,dll)
# - fungsi values() -> menampilkan seluruh nilai pada data
# - fungsi columns() -> menampilkan seluruh kolom

# %% [markdown]
# ### Inspecting DataFrame

# %%
import pandas as pd

homeless = pd.read_csv("homelessness.csv")

# %%
homeless.head() #menampilkan beberapa data pertama

# %%
homeless.info() #menampilkan informasi dataframe

# %%
homeless.shape #menampilkan jumlah baris dan kolom

# %%
homeless.describe() #menampilkan statistik dataframe

# %%
homeless.values

# %%
homeless.columns

# %%
homeless.index

# %% [markdown]
# ### Sorting and subsetting

# %% [markdown]
# - fungsi sort_values -> mengurutkan nilai
# - ascending (dari kecil ke besar)
# - nama_dataframe["kolom"]
# - nama_dataframe[["kolom1", "kolom2"]]
# - nama_dataframe["kolom"] kondisi -> hasilnya true, false
# - nama_dataframe[nama_dataframe["kolom"] kondisi]

# %%
import pandas as pd

homeless = pd.read_csv("homelessness.csv")

# %%
homeless

# %%
homeless_ind = homeless.sort_values("individuals")

# %%
homeless_ind

# %%
homeless_ind.head()

# %%
homeless_fam = homeless.sort_values("family_members", ascending=False)

# %%
homeless_fam

# %%
homeless_fam.head()

# %%
homeless_reg_fam = homeless.sort_values(["region", "family_members"], ascending=[True, False])

# %%
homeless_reg_fam.head()

# %%
individuals = homeless[["individuals"]]

# %%
individuals

# %%
individuals.head()

# %%
state_fam = homeless[["state", "family_members"]]

# %%
state_fam.head()

# %%
ind_state = homeless[["individuals", "state"]]

# %%
ind_state.head()

# %%
ind_gt_10k = homeless[homeless["individuals"] > 10000]

# %%
ind_gt_10k

# %%
mountain_reg = homeless[homeless["region"] == "Mountain"]

# %%
mountain_reg

# %%
fam_lt_1k_pac = homeless[(homeless["family_members"] < 1000) & (homeless["region"] == "Pacific")]

# %%
fam_lt_1k_pac

# %%



