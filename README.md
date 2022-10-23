#include <iostream>
#include <ctime>
#include <cstdlib>
#include <string>
#include <fstream>
#include <vector>


using namespace std;

class Person {
protected:
	int health = 0;
	int healthMax = 0;
	int energy = 0;
	int level = 0;
	string name = "";
public:
	Person(int health, int energy, int level, string name) {
		this->health = health;
		this->healthMax = health;
		this->energy = energy;
		this->level = level;
		this->name = name;
	}

	void setHealth(int health) {
		if (health > this->healthMax) {
			this->health = this->healthMax;
		}
		else {
			this->health = health;
		}
	}

	int getHealth(int health) {
		return health;
	}

	int getLevel() {
		return this->level;
	}
};

class Armor {
protected:
	int protection = 0;
	string name = "";
	int price = 0;
public:
	Armor(int protection, string name) {
		this->protection = protection;
		this->name = name;
		this->price = protection*30;
	}

	string getName() {
		return this->name;
	}

	int getProtection() {
		int protection = 3 + rand() % 18;
		return protection;
	}

	int getPrice() {
		return this->price;
	}
};

class Weapon {
protected:
	int weaponDamage = 0;
	string name;
	int price = 0;
public:
	Weapon(int weaponDamage, string name) {
		this->weaponDamage = weaponDamage;
		this->price = weaponDamage * 30;
	}

	int getDamage() {
		this->weaponDamage = 3 + rand() % 28;
		return this->weaponDamage;
	}

	int getPrice() {
		return this->price;
	}

	string getName() {
		return this->name;
	}

};

class Player : public Person {
protected:
	int power = 20;
	int agility = 20;
	int endurance = 20;
	int experience = 0;
	int money = 200;
	int damage = 0;
	int protection = 0;
	Weapon* weapon = NULL;
	Armor* armor = NULL;
public:
	Player(int health, int energy, int level, string name, int power, int agility, int endurance) : Person(health, energy, level, name) {
		this->power = power;
		this->agility = agility;
		this->endurance = endurance;
		this->experience = experience;
		this->money = money;
	}

	void setWeapon(Weapon* weapon) {
		this->weapon = weapon;
	}

	void setArmor(Armor* armor) {
		this->armor = armor;
	}

	void setMoney(int money) {
		this->money=money;
	}

	int setPower(int power, int prof) {
		if (prof == 1) {
			power *= 2;
		}
		this->power = power;
		return this->power;
	}

	int getPower() {
		return this->power;
	}

	int setEndurance(int endurance, int prof) {
		if (prof == 2) {
			endurance *= 2;
		}
		return this->endurance;
	}

	int getEndurance() {
		return this->endurance;
	}

	int getPlayerDamage() {
		return this->damage;
	}

	int getProtection() {
		return this->protection;
	}

	int setPlayerDamage(int weaponDamage, int prof) {
		this->damage = (weaponDamage * setPower(getPower(), prof)) % 100;
		return this->damage;
	}

	int setProtection(int armorProtection, int prof) {
		int protection = (setEndurance(getEndurance(),prof) * armorProtection) % 100;
		return this->protection;
	}

	int getDefaultLv() {
		int level = 1;
		return level;
	}

	int getMoney() {
		return this->money;
	}

	int getExperiance() {
		return this->experience;
	}

	int spendMoney(int price) {
		int money = getMoney() - price;
		return money;

	}

	int getAgility(int prof) {
		int agility = 20;
		if (prof == 3) {
			agility *= 2;
		}
		return agility;
	}

	int getHealing() {
		int healdHP = rand() % 15;
		return healdHP;
	}

	int getLvlUp(int experiance, int level, int energy, int power, int agility, int healthMax) {
		if (experiance >= 1000) {
			level += 1;
			experiance -= 1000;
			healthMax += 100;
			energy *= 2;
			power += 40;
			agility += 60;
		}
		return level;
	}
};

class Monster : public Person {
protected:
	int damage = 20;
	int armor = 0;
	int plwonpoints = 0;
	int money = 100;
public:
	Monster(int health, int energy, int level, string name, int damage, int armor, int plwonpoints) : Person(health, energy, level, name) {
		this->damage = damage;
		this->armor = armor;
		this->plwonpoints = plwonpoints;
		this->money = level * money;
	}

	int getDamage() {
		int damage = 20;
		return damage;
	}

};

class Engine {
private:
	string monsterName[10] = { "Kitana","Fearless Hannah","Cyborg killer","Bloodsucker","Scary Dracula","Roberto the shooter","Witcher","Your math teacher","russians","Volondemort" };
	string armorName[7] = { "Box", "Stone shield", "Iron shield", "Knight armour","Siver sheild","Ruby armor" };
	string weaponName[7] = { "Fork","Knife","Ordinary sword","Silver sword","Ax","Fireballs","Crystal sword" };
	Player* info = NULL;
	Monster* monsterInfo = NULL;
	
	int random(int min, int max)
	{
		if (min > max) {
			swap(min, max);
		}

		return min + rand() % (max - min + 1);
	}

public:
	Armor* generateArmor(Armor* armorInfo) {
		string name = armorName[random(0, 6)];
		return new Armor(armorInfo->getProtection(), name);
	}

	Weapon* generateWeapon(Weapon* weaponInfo) {
		string name = weaponName[random(0, 6)];
		return new Weapon(weaponInfo->getDamage(), name);
	}


	Player* createPlayer(string name, int prof, int protection, int playerDamage) {
		return new Player(20 * info->setEndurance(info->getEndurance(),prof), 30, 1, name, info->setPower(info->getPower(),prof), info->getAgility(prof), info->setEndurance(info->getEndurance(),prof));
	}

	Monster* generateMonster(int level) {
		if (level > 1) {
			if (rand() % 3 == 1) {
				level++;
			}
			else if (rand() % 3 == 2) {
				level--;
			}
		}
		else {
			if (rand() % 2 == 1) {
				level++;
			}
		}

		int health = (100 * level) + random(5, 50), energy = (20 * level) + random(5, 25), damage = (10 * level) + random(1, 12), armor = (10 * level) + random(1, 12), plwonpoints = (100 * level) + random(20, 120);

		return new Monster(health, energy, level, this->monsterName[rand() % 10], monsterInfo->getDamage() + (10 * level), armor, plwonpoints);
	}
	void criticalHit(int prof) {
		if (random(1, 4) == 3) {
			cout << "Critical hit! WOW! Your damage is " << info->setPlayerDamage(info->getPlayerDamage(),prof) * 2 << endl;
		}
	}

	void fight(Player* player, Monster* mob, Weapon* weaponInfo,int prof) {
		int playerHealth = player->getHealth(300);
		int monsterHealth = mob->getHealth(350);
		cout << "3 2 1 FIGHT!\n";
		for (int i = 1; i < 5; i++) {
			playerHealth -= monsterInfo->getDamage() + info->getHealing();
			monsterHealth -= player->setPlayerDamage(weaponInfo->getDamage(), prof);
			cout << i << "raund: " << " Your damage:" << info->setPlayerDamage(weaponInfo->getDamage(), prof) << " Your HP:" << playerHealth << endl;
			cout << "Monster damage: " << monsterInfo->getDamage() << " Monster HP: " << monsterHealth << endl;
		}
		if (playerHealth > monsterHealth) {
			cout << "You won! Congrats!\n";
		}
		else if (playerHealth < monsterHealth) {
			cout << "You lose :(\n";
		}
		else {
			cout << "Draw\n";
		}
		cout << "\n============================================\n";
	}
};

class Event {
private:
	Engine* engine = NULL;
	Player* info = NULL;
	Weapon* weaponInfo = NULL;
	Armor* armorInfo = NULL;

	bool checkPlayerMoney(int cash, int price) {
		if (price <= cash) {
			return true;
		}

		return false;
	}
public:
	Event(Engine* engine, Player* player, Weapon* weaponInfo, Armor* armorInfo) {
		this->engine = engine;
		this->info = player;
		this->weaponInfo = weaponInfo;
		this->armorInfo = armorInfo;
	}

	void shop(Player* player) {
		int answer;
		cout << "Welcome to the shop, where you can buy weapon-1 or armor-2. What do you want to buy?\n";
		cin >> answer;

		if (answer == 1) {
			vector<Weapon*> weaponList;

			for (int i = 0; i < 3 + rand() % 5; i++) {
				weaponList.push_back(engine->generateWeapon(this->weaponInfo));
			}

			for (int i = 0; i < weaponList.size(); i++) {
				cout << i + 1 << " " << weaponList[i]->getName() << " " << weaponList[i]->getPrice() << endl;
			}

			int ch = 0;
			cout << "What do you want?\n";
			cin >> ch;

			if (this->checkPlayerMoney(player->getMoney(), weaponList[ch - 1]->getPrice())) {
				player->setWeapon(weaponList[ch - 1]);
				player->setMoney(player->getMoney() - weaponList[ch - 1]->getPrice());
			}
			else {
				cout << "You do not have much money, so you can not buy it!";
			}
		}
		else if (answer==2) {
			vector<Armor*> armorList;

			for (int i = 0; i < 3 + rand() % 5; i++) {
				armorList.push_back(engine->generateArmor(this->armorInfo));
			}

			for (int i = 0; i < armorList.size(); i++) {
				cout << i + 1 << " " << armorList[i]->getName() << " " << armorList[i]->getPrice() << endl;
			}

			int ch = 0;
			cout << "What do you want?\n";
			cin >> ch;

			if (this->checkPlayerMoney(player->getMoney(), armorList[ch - 1]->getPrice())) {
				player->setArmor(armorList[ch - 1]);
				player->setMoney(player->getMoney() - armorList[ch - 1]->getPrice());
			}
			else {
				cout << "You do not have much money, so you can not buy it!";
			}
		}

		else {
			cout << "You made a mistake, please enter another number.";
		}
	}

	void meetingMonster() {
		cout << "Hey, remember my face, because you are going to see it again in your nightmares after our bloody fight! AhAhAhAh!!!\n";
	}

	void powerUp() {
		cout << "You was going through the dark forest and found out something strange! And it was... A bowl with the most delicious meal in the world - Ukrainian Borsch! You ate it so now you have double power, congrats!\n";
	}

	void agilityUp() {
		cout << "You was going through the dark forest and heard the steps. When you turned around you saw... A small russian. You started running how fast as you can and learned how to run better than you ever could. So now you have double agility, congrats!\n";

	}

	void enduranceUp() {
		cout << "You was going through the dark forest and you steped on something really bad... You steped on the portrait of putin. You started to fight it, until it desapears. So now you have double endurance, congrats!\n";
	}
};

class SaveLoad {
public:
	void save(Player* obj) {
		ofstream save;

		save.open("gameSave.txt");

		save.write((char*)&obj, sizeof(Player));

		save.close();
	}

	Player* load() {
		Player* person = NULL;
		ifstream load;
		load.open("gameSave.txt");

		load.read((char*)&person, sizeof(Player));

		load.close();

		return person;
	}
};

int main()
{
	srand(static_cast<unsigned int>(time(0)));
	string playerName = "";
	int answer;
	Engine* engine = new Engine();
	Player* user = NULL;
	Armor* armor = NULL;
	Weapon* weapon = NULL;
	Event* event = new Event(engine,user,weapon,armor);
	cout << "Enter your player name:\n";
	cin >> playerName;


	cout << "Welcome to my game, where you will need to fight with the monster. Good luck :) \n";
	cout << "Who you want to be? 1 - barbarian, 2 - tank, 3 - robber.\n";
	cin >> answer;

	if (answer > 3 || answer < 1) {
		cout << "You made a mistake, please enter another number.";
		return 0;
	}

	armor = engine->generateArmor(armor);
	weapon = engine->generateWeapon(weapon);
	user = engine->createPlayer(playerName, answer, user->setProtection(armor->getProtection(),answer), user->setPlayerDamage(weapon->getDamage(), answer));

	for (int i = 0; i < 5; i++) {
		int n = rand() % 4;
		int n1 = 0;
		if (rand() % 5 == 2 || rand() % 5 == 3) {
			Monster* monster = engine->generateMonster(user->getLevel());
			engine->fight(user, monster,weapon,answer);
		}
		else if (n1==0) {
			event->shop(user);
		}
		else if (rand() % 4 == 3) {
			if (n == 1) {
				event->agilityUp();
			}
			else if (n == 2) {
				event->enduranceUp();
			}
			else {
				event->powerUp();
			}
		}
	}
	return 1;
}