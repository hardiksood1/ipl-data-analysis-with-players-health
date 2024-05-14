import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
match_data = pd.read_csv(r'C:\Users\hp\Desktop\matches.csv')
delivery_data = pd.read_csv(r'C:\Users\hp\Desktop\deliveries.csv.csv')
match_data.head()
match_data.head()
match_data.isnull().sum()
delivery_data.isnull().sum()
match_data.shape
delivery_data.shape
print('match played so far :' , match_data.shape[0])
print('\n city played at :', match_data['City'].unique())
print('\n teams participated :',match_data['Team1'].unique())
match_data['Season'] = pd.DatetimeIndex(match_data['Date']).year
match_data.head()
match_per_season = match_data.groupby(['Season'])['ID'].count().reset_index().rename(columns = {'ID': 'matches'})
match_per_season
sns.histplot(match_data['Season'],bins=15)
plt.xticks( fontsize=10)
plt.yticks(fontsize=15)
plt.xlabel('Season', fontsize=10)
plt.ylabel('matches', fontsize=10)
plt.title('total matches played in each Season', fontsize = 10, fontweight="bold")
Season_data = match_data[['ID','Season']].merge(delivery_data,left_on='ID',right_on = 'ID', how ='left').drop('ID',axis = 1)
Season_data.head()
season =Season_data.groupby(['Season'])['total_run'].sum().reset_index()
p = season.set_index('Season')
ax = plt.axes()
ax.set(facecolor = "grey")
sns.lineplot(data=p,palette="magma")
plt.title('total run in each season',fontsize=12,fontweight="bold")
plt.show()
runs_per_season = pd.concat([match_per_season,season.iloc[:,1]],axis=1)
runs_per_season['Run scored per match']=runs_per_season['total_run']/runs_per_season['matches']
runs_per_season.set_index('Season',inplace=True)
runs_per_season
toss=match_data['TossWinner'].value_counts()
ax = plt.axes()
ax.set(facecolor = "grey")
sns.set(rc = {'figure.figsize' : (15,10)},style='darkgrid')
ax.set_title('No. of tosses won by eatch team ',fontsize=15, fontweight="bold")
sns.barplot(y=toss.index, x=toss, orient='h',palette = "icefire", saturation=1)
plt.xlabel(' of tosses won')
plt.ylabel('Team')
plt.show()
ax = plt.axes()
ax.set(facecolor="grey")
sns.countplot(x='Season', hue='TossDecision', data = match_data,palette="magma",saturation=1)
plt.xticks(rotation=90,fontsize=10)
plt.yticks(fontsize=15)
plt.xlabel('\n Season',fontsize=15)
plt.ylabel('count',fontsize=15)
plt.title('Toss decision across seasoan', fontsize=12,fontweight="bold")
plt.show()
match_data['WonBy'].value_counts()
match_data.Venue[match_data.WonBy!='Runs'].mode()
match_data.Venue[match_data.WonBy!='Wickets'].mode()
filtered_data = match_data[(match_data['TossWinner'] == 'Rajasthan Royals') & ((match_data['Team1'] == 'Rajasthan Royals') | (match_data['Team2'] == 'Gujarat Titans'))]
mode_venue = filtered_data['Venue'].mode()
print(" won the toss and played:")
print(mode_venue)
match_data.WinningTeam[match_data.WonBy!='Runs'].mode()
match_data.WinningTeam[match_data.WonBy!='Wickets'].mode()
toss1 = match_data['TossDecision'] == match_data['WinningTeam']
#plt.figutre = (15,10)
#sns.histplot(toss)
#plt.show()
sns.histplot(toss1)
#plt.xticks( fontsize=10)
#plt.yticks(fontsize=15)
plt.xlabel('toss')
plt.ylabel('frequecy')
plt.title('total matches played in each Season', fontsize = 10, fontweight="bold")
toss.tail()
numeric_data = [1 if x else 0 for x in toss]
sns.histplot(numeric_data, bins=2, color="blue", edgecolor = "black")
plt.figure(figsize = (12,4))
sns.countplot(match_data.TossDecision[match_data.TossWinner == match_data.WinningTeam])
plt.show()
player = (delivery_data['batter']=='JC Buttler')
df_Buttler = delivery_data[player]
df_Buttler.head()
df_Buttler['kind'].value_counts().plot.pie(autopct='%1.1f%%',shadow=True,rotatelabels=True)
plt.title("Player Out", fontweight="bold",fontsize=15)
plt.show()
delivery_data['fitness_score']= 2*delivery_data['batsman_run']+0.5*delivery_data['ballnumber']
fitness_threshold = 5
for index, player in delivery_data.iterrows():
    if player['fitness_score'] >= fitness_threshold:
        print(f"{player['batter']} is fit with a fitness score of {player['fitness_score']}.")
    else:
        print(f"{player['batter']} needs to work on fitness with a score of {player['fitness_score']}.")
selected_player = 'JC Buttler'
player_data = delivery_data[delivery_data['batter'] == selected_player]
plt.figure(figsize=(12, 6))
plt.plot(player_data['fitness_score'], marker='o', color='b', linestyle='-')

plt.title(f'Fitness Score of' )
plt.xlabel('Balls Faced')
plt.ylabel('Fitness Score')
plt.grid(True)
plt.show()
match_data['Margin'] = pd.to_numeric(match_data['Margin'], errors='coerce')
match_data['Win_Ratio'] = match_data['WinningTeam'] / match_data['Margin']
print(match_data['Win_Ratio'])
plt.figure(figsize=(10, 6))
plt.bar(match_data['Team2'], match_data['Win_Ratio'], color='skyblue')
plt.xlabel('Team2')
plt.ylabel('Winning Ratio')
plt.title('Winning Ratio of Teams')
plt.ylim(0, 1)  # Set y-axis limits to show ratios between 0 and 1
plt.show()
class Player:
    def __init__(self, name, health):
        self.name = name
        self.health = health

    def take_damage(self, damage):
        self.health -= damage
        if self.health >= 80:
            print("Player is Fit")
        elif 60 <= self.health < 80:
            print("Player is Moderate damage")
        else:
            print("Player is high damage")

    def show_health(self):
        print(f"{self.name}'s health is {self.health}")

player1 = Player("JC Buttler", 100)

damage_taken = 20
player1.take_damage(damage_taken)

player1.show_health()
class Player:
    def __init__(self, name, health):
        self.name = name
        self.health = health

    def take_damage(self, damage):
        self.health -= damage
        if self.health >= 80:
            print("Player is Fit")
        elif 60 <= self.health < 80:
            print("Player is Moderate damage")
        else:
            print("Player is high damage")

    def show_health(self):
        print(f"{self.name}'s health is {self.health}")

player1 = Player("JC Buttler", 100)

damage_taken = 20
player1.take_damage(damage_taken)

player1.show_health()
def plot_health_history(self):
        plt.plot(range(len(self.health_history)), self.health_history, marker='o')
        plt.xlabel('Damage Taken')
        plt.ylabel('Health')
        plt.title(f'{self.name}\'s Health History')
        plt.show()
player1 = Player("JC Buttle", 100)
damage_taken = [10, 20, 30, 15]
for damage in damage_taken:
    player1.take_damage(damage)
    player1.show_health()
