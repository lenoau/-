import os
import pandas as pd

df_total = pd.dataframe()

def read_files_from_dir(start_dir):
	df_all = pd.DataFrame()
	file_names = os.listdir(start_dir)
	for file_name in file_names:
		if'DatLotResult_PC2' in file_name:
			full_file_path = os.path.join(start_dir, file_name)
			df = pd.read_csv(full_file_path, delimiter=/'t)
			df_all = pd.concat([df, df_all])
	return df_all


nametag = {'10.15.118.61':{'name': 'C5_DFA_1,'path':'eq_09_1}}

start_date = pd.to_datetime('2024-1-21')
end_date = pd.to_datetime('2024-1-21')

dates = pd.date_range(start_date, end_date)

dfs = {ip: pd.dateframe() for ip in nametag.keys()}

for date in dates:
	year = date.year
	month = date.month
	day = date.day
	
	for ip, info in nametag.items():
		start_dir = f"//{info['path']/CMI_Results/{year}/{month}/{day}"
		df_all = read_files_from_dir(start_dir)
		dfs[ip] = pd.concat([dfs[ip], df_all])

		save_folder_path = f"d:/류승진/DFA AVI 치수 데이터/{start_date.strftime(%Y-%m-%d)}_to_{end_date.strftime(%Y-%m-%d)}"
		os.makedirs(save_folder_path, exist_ok=True)
		dfs[ip].to_excel(f"{save_folder_path}/{info['namee']}_치수 데이터.xlsx", index=False)

		print(dfs[ip])
