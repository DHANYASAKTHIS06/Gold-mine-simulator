import time 
class GameEvents:  
def init (self): 
self.counter = 0 
def handle(self, gold, workers, tools, day):  self.counter += 1 
bonus = 0  
cavein = False  
tool_loss = False 
double_next = False 
if day % 5 == 0: 
bonus += 100 
print("Event: +100 gold (Bonus Day)") 
if day % 6 == 0 and workers > 0:  
workers -= 1 
print("Event: Cave-in! Lost 1 worker.")
  
if day % 7 == 0 and tools > 1:  
tools -= 1 
print("Event: Toolsrusted! Lost 1 tool.") 
if day % 10 == 0:  
double_next = True 
print("Event: Rich Vein! Next mining is doubled.") 
gold += bonus 
return gold, workers, tools, double_next 
class GoldMineGame:  
def init (self): 
self.gold = 100 
self.workers = 2 
self.tools = 1 
self.depth = 1 
self.fatigue = 0 
self.day = 1 
self.max_days = 20 
self.goal = 10000 
self.double_next = False
  
self.performance = [] 
self.events = GameEvents()  
self.logs = [] 
self.inventory = {"food": 5, "water": 5}  
self.day_times = [] 
def show_status(self): 
print(f"\nDay {self.day} | Gold: {self.gold} | Workers: {self.workers} | Tools: {self.tools} | Depth: {self.depth} |  Fatigue: {self.fatigue}") 
print(f"Inventory -> Food: {self.inventory['food']}, Water: {self.inventory['water']}") 
def mine(self): 
efficiency = 0.7 ifself.fatigue >= 5 else 1.0  
multiplier = 2 if self.double_next else 1 
earned = int(10 * self.workers * self.tools * self.depth *  efficiency * multiplier) 
self.gold += earned  
self.fatigue += 1 
self.double_next = False 
print(f"Mined {earned} gold.") 
self.logs.append(f"Day {self.day}: Mined {earned} gold.")
 
def hire_worker(self):  
if self.gold >= 50: 
self.workers += 1 
self.gold -= 50 
print("Hired 1 worker.") 
self.logs.append(f"Day {self.day}: Hired 1 worker.")  else: 
print("Not enough gold.") 
def upgrade_tools(self):  
if self.gold >= 100:  
self.tools += 1 
self.gold -= 100 
print("Upgraded tools.") 
self.logs.append(f"Day {self.day}: Upgraded tools.")  else: 
print("Not enough gold.") 
def expand_mine(self):  
if self.gold >= 200:  
self.depth += 1

self.gold -= 200 
print("Expanded mine.") 
self.logs.append(f"Day {self.day}: Expanded mine.")  else: 
print("Not enough gold.") 
def rest(self): 
ifself.inventory['food'] > 0 and self.inventory['water'] > 0:  self.inventory['food'] -= 1 
self.inventory['water'] -= 1 
self.fatigue = max(0,self.fatigue - 2)  
print("Rested. Fatigue reduced.") 
self.logs.append(f"Day {self.day}: Rested.")  
else: 
print("Not enough food or water to rest.") 
def visit_shop(self): 
print("\n--- Shop ---") 
print("1. Food (10 gold) 2. Water (8 gold) 3. Back")  choice = input("Buy: ").strip() 
try: 
if choice == '1' and self.gold >= 10:
 
self.inventory['food'] += 1 
self.gold -= 10 
print("Bought 1 food.") 
elif choice == '2' and self.gold >= 8:  
self.inventory['water'] += 1 
self.gold -= 8 
print("Bought 1 water.")  
elif choice == '3': 
return  
else: 
print("Not enough gold or invalid choice.")  except Exception as e: 
print("Error:", e) 
def show_progress(self): 
print("\n--- Gold Progress Chart ---") 
fori, gold in enumerate(self.performance, 1):  bar_length = gold // 100 
bar = '*' * bar_length 
print(f"Day {i:2}: {bar} ({gold} gold)")  
def end_summary(self):
print("\n--- Game Summary ---")  
for log in self.logs: 
print(log) 
print("\n--- Time Per Day ---") 
fori, t in enumerate(self.day_times, 1):  
print(f"Day {i:2}: {t} seconds") 
def play(self): 
print("Welcome to Gold Mine Simulator!") 
while self.day <= self.max_days and self.gold < self.goal:  day_start = time.time() 
self.show_status() 
print("1-Mine\n2-Hire\n3-Upgrade\n4-Expand\n5- Rest\n6-Shop\n7-Exit") 
try: 
choice = input("Choose action: ").strip() 
if choice == '1':  
self.mine()  
elif choice == '2': 
self.hire_worker()  
elif choice == '3': 
self.upgrade_tools()  
elif choice == '4': 
self.expand_mine()  
elif choice == '5': 
self.rest() 
elif choice == '6':  
self.visit_shop() 
elif choice == '7': 
print("\nYou exited the game early.")  
break 
else: 
raise ValueError("Invalid input. Please choose  between 1 to 7.") 
except Exception as e:  
print("Error:", e) 
self.gold,self.workers,self.tools,self.double_next =  self.events.handle( 
self.gold, self.workers, self.tools, self.day 
)
self.performance.append(self.gold) 
day_end = time.time() 
elapsed = int(day_end - day_start)  
self.day_times.append(elapsed) 
self.day += 1 
ifself.gold >= self.goal: 
print("\nVictory! You reached the gold goal.")  elif self.day > self.max_days: 
print(f"\nGameOver. Final Gold: {self.gold}")  else: 
print(f"\nGame Ended Early. Final Gold: {self.gold}") 
self.show_progress()  
self.end_summary() 
game = GoldMineGame()  
game.play()
