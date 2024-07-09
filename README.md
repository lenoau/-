# -
LG이노텍 근무시절 DFA AVI 호기별 치수 불량 코드 추출

import os
import pandas as pd

df_total = pd.date.frame()

def read_files_from_dir(start_dir):
	df_all = pd.DateFrame()
	for dir_path, dir_names, file_names in
os.walk(start_dir):
	for file_name in file_names:
		if 'NG' in file_name:
			full_file_path = os.path.join(dir_path, file_name)
			df = pd.read_csv(full_file_path)
			df['LOT ID']= file_name.replace('.csv',").replace('_NG',")
			df_all = pd.concat([df, df_all], ignore_index=True)
	if df_all.empty:
		return df_all
	cols = df_all.columns.tolist()
	cols.insert(0, cols.pop(cols.index('Lot ID')))
	df_all = df_all.reindex(columns = cols)
	df_all = df_all{'MN-code'].str.contains('DFAI', case=Faise, na=False)]
	return df_all

nametag = {'10.15.118.54': 'C5_DFA_1'}

start_date = pd.to_datetime('2024-01-10')
end_date = pd.to_datetime('2024-01-15')

dates = pd.date_range(start_date, end_date)

dfs = {ip: pd.dataframe() for ip in nametag.keys()}

for ip, name in nametag.items():
	for date in dates:
		date = date.strftime('%y-%m-%d')
		year, month, day = date.split('_')
		start_dir = f"//{ip}/CMAI2100_handler/LOG/OutTray/{yaer}-[Month}-{day}"
		df_all = read_files_from_dir(start_dir)
		dfs[ip] = pd.concat([dfs[ip], df_all], ignore_index=True,)
	counts = dfs[ip]['MN-code'].value_counts()
	total = dfs[ip]['MN-Code'].count()
	percentages = counts / total
	df_counts = pd.DataFrame({'MN-Code': counts.index, 'count': counts.values, 'percentages':percentages.values}).reset_index(drop=True)
	dfs[ip]= pd.concat([dfs[ip], df_counts], axis=1)

	save_folder_path = f"d:/류승진/DFA AVI 치수 불량/{start_date.strftime(%Y-%m-%d)}_to_{end_date.strftime(%Y-%m-%d)}"
		os.makedirs(save_folder_path, exist_ok=True)
		dfs[ip].to_excel(f"{save_folder_path}/{info['name']}_치수 불량.csv", index=False)

		print(dfs[ip])
