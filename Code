# Import Libraries
import pandas as pd
import matplotlib.pyplot as plt
import datetime as dt
#Read Data from schedule.csv
df =pd.read_csv('schedule.csv')
df.head()
###### PRE-PROCESSING THE DATA ######
#Convert dates to datetime format
df.start=pd.to_datetime(df.start)
df.end=pd.to_datetime(df.end)
#Add Duration
df['duration']=df.end-df.start
df.duration=df.duration.apply(lambda x: x.days+1)
#sort in ascending order of start date
df=df.sort_values(by='start', ascending=True)
#project level variables
p_start=df.start.min()
p_end=df.end.max()
p_duration=(p_end-p_start).days+1
#Add relative date
df['rel_start']=df.start.apply(lambda x: (x-p_start).days)
#Create custom x-ticks and x-tick labels
x_ticks=[i for i in range(p_duration+1)]
x_labels=[(p_start+dt.timedelta(days=i)).strftime('%d-%b')
          for i in x_ticks]
######  PLOTTING GANTT CHART ######
plt.figure(figsize=(8,4))
plt.title('Basic Gantt Chart', size=18)
plt.barh(y=df.Task, left=df.rel_start, width=df.duration)
plt.gca().invert_yaxis()
plt.xticks(ticks=x_ticks[::3], labels=x_labels[::3])
plt.grid(axis='x')
plt.show()

df['w_comp']=round(df.Completion*df.duration/100,2)
df.head()

plt.figure(figsize=(8,4))
plt.title('Gantt Chart:Completion', size=18)
#Light bar for entire task
plt.barh(y=df.Task, left=df.rel_start, width=df.duration,
         alpha=0.4, color='green')
#Darker bar for completed part
plt.barh(y=df.Task, left=df.rel_start, width=df.w_comp,
         alpha=1, color='green')
plt.gca().invert_yaxis()
plt.xticks(ticks=x_ticks[::3], labels=x_labels[::3])
plt.grid(axis='x')
plt.show()

c_dict={'Mkt':'red', 'Fin':'green', 'HR':'blue'}

plt.figure(figsize=(8, 4))
plt.title('Gantt Chart:Completion | Dept.', size=18)
for i in range(df.shape[0]):
    color = c_dict[df.Department[i]]
    plt.barh(y=df.Task[i], left=df.rel_start[i],
             width=df.duration[i], alpha=0.4,
             color=color)
    plt.barh(y=df.Task[i], left=df.rel_start[i],
             width=df.w_comp[i], alpha=1, color=color)

plt.gca().invert_yaxis()
plt.xticks(ticks=x_ticks[::3], labels=x_labels[::3])
plt.grid(axis='x')
plt.show()

#Sort based on Department
df=df.sort_values(by='Department',
                  ascending=False).reset_index(drop=True)

plt.figure(figsize=(8,4))
plt.title('Gantt Chart | Completion% | Sorted by Dept', size=18)
for i in range(df.shape[0]):
    color=c_dict[df.Department[i]]
    plt.barh(y=df.Task[i], left=df.rel_start[i],
             width=df.duration[i], alpha=0.4,
             color=color)
    plt.barh(y=df.Task[i], left=df.rel_start[i],
             width=df.w_comp[i], alpha=1, color=color)
plt.gca().invert_yaxis()
plt.xticks(ticks=x_ticks[::3], labels=x_labels[::3])
plt.grid(axis='x')
plt.show()

yticks=[i for i in range(len(df.Task))]
plt.figure(figsize=(8, 4))
plt.title('Gantt Chart | Completion%', size=18)
for i in range(df.shape[0]):
    color = c_dict[df.Department[i]]
    plt.barh(y=df.Task[i], left=df.rel_start[i],
             width=df.duration[i], alpha=0.4,
             color=color)
    plt.barh(y=df.Task[i], left=df.rel_start[i],
             width=df.w_comp[i], alpha=1, color=color)
    plt.text(x=df.rel_start[i] + df.w_comp[i],
             y=yticks[i],
             s=f'{df.Completion[i]}%')

plt.gca().invert_yaxis()
plt.xticks(ticks=x_ticks[::3], labels=x_labels[::3])
plt.grid(axis='x')
plt.show()

plt.figure(figsize=(12, 7))
plt.title('Gantt Chart:Project Mayhem', size=18)
for i in range(df.shape[0]):
    color = c_dict[df.Department[i]]
    plt.barh(y=yticks[i], left=df.rel_start[i],
             width=df.duration[i], alpha=0.4,
             color=color)
    plt.barh(y=yticks[i], left=df.rel_start[i],
             width=df.w_comp[i], alpha=1, color=color,
             label=df.Department[i])
    plt.text(x=df.rel_start[i] + df.w_comp[i],
             y=yticks[i],
             s=f'{df.Completion[i]}%')

plt.gca().invert_yaxis()
plt.xticks(ticks=x_ticks[::3], labels=x_labels[::3])
plt.yticks(ticks=yticks, labels=df.Task)
plt.grid(axis='x')
# fix legends
handles, labels = plt.gca().get_legend_handles_labels()
handle_list, label_list = [], []
for handle, label in zip(handles, labels):
    if label not in label_list:
        handle_list.append(handle)
        label_list.append(label)
plt.legend(handle_list, label_list, fontsize='medium',
           title='Department', title_fontsize='large')
plt.show()
