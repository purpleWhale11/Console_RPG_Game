#include <iostream>
#include <ctime>
#include <cstdlib>
#include <string>
#include <fstream>
#include <vector>
#include <conio.h>


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

	int getHealth() {
		return this->health;
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
		this->price = protection*15;
	}

	string getName() {
		return this->name;
	}

	int getProtection() {
		int protection = 3 + rand() % 28;
		return protection;
	}

	int getPrice() {
		return this->price;
	}
};

class Weapon {
protected:
	int weaponDamage=0;
	string name;
	int price = 0;
public:
	Weapon(int weaponDamage, string name) {
		this->weaponDamage = weaponDamage;
		this->name = name;
		this->price = weaponDamage * 15;
	}

	int getDamage() {
		int weaponDamage = 3 + rand() % 28;
		return weaponDamage ;
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
	int money = 500;
	int damage = 100;
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

	int setPower(int prof) {
		int power = 20;
		if (prof == 1) {
			power = (20 + rand() % 6) * 2;
		}
		return power;
	}

	int setEndurance(int prof) {
		int endurance = 20;
		if (prof == 2) {
			endurance *= 2;
		}
		return endurance;
	}

	int setAgility(int prof) {
		int agility = 20;
		if (prof == 3) {
			agility *= 2;
		}
		return agility;
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
		int damage = (weaponDamage * setPower(prof)) % 100;
		return damage;
	}

	int setProtection(int armorProtection, int prof) {
		int protection = (setEndurance(prof) * armorProtection) % 100;
		return protection;
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

	int getAgility() {
		return this->agility;
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

	int getHealthMax() {
		return this->healthMax;
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

class SkillInterface {
public:
	virtual void use(Player* player) = 0;
};

class HardHit : public SkillInterface {
public:
	void use(Player* player) override {
		player->setPlayerDamage(player->getPlayerDamage() + 100, 1);
		cout << "I'm using the HardHit skill. Player damage = " << player->getPlayerDamage()+100 << endl;
	}
};

class OrdinaryHit : public SkillInterface {
public:
	void use(Player* player) override {
		player->setPlayerDamage(player->getPlayerDamage() + 100, 1);
		cout << "I'm usinig the OrdinaryHit skill. Player damage = " << player->getPlayerDamage()+100 << endl;
	}
};

class Healing : public SkillInterface {
private:
	int size = 0;
public:
	Healing() {
		this->size = rand() % 3;
	}

	void use(Player* player) override {
		int healthValue = 10;
		if (this->size == 1) {
			healthValue = 20;
		}
		else if (this->size == 2) {
			healthValue = 50;
		}
		else if (this->size == 3) {
			healthValue = 100;
		}
		player->setHealth(player->getHealth() + healthValue);
		cout << "I'm using the Healing skill. Health = " << player->getHealth()+healthValue << endl;
	}
};

class PotionInterface {
public:
	virtual void drink(Player* player) = 0;
};

class HpPotion : public PotionInterface {
private:
	int size = 0;
	int price = 0;
public:
	HpPotion() {
		this->size = rand() % 4;
		this->price = 150 + rand() % 251;
	}

	void drink(Player* player) override {
		if (size != 4) {
			int healthValue = 10;
			if (this->size == 2) {
				healthValue = 50;
			}
			else if (this->size == 3) {
				healthValue = 100;
			}

			player->setHealth(player->getHealth() + healthValue);
		}
		else {
			player->setHealth(player->getHealthMax());
		}
		cout << "I drunk hp potion. HP = " << player->getHealth() << endl;
	}

	int getPrice() {
		return this->price;
	}
};

class EndurancePotion : public PotionInterface {
private:
	int price = 0;
public:
	EndurancePotion() {
		this->price = 150 + rand() % 251;
	}

	void drink(Player* player) override {
		player->setEndurance(player->getEndurance() + 20);
		cout << "I drunk endurance potion. Endurance = " << player->getEndurance() << endl;
	}

	int getPrice() {
		return this->price;
	}
};

class AgilityPotion : public PotionInterface {
private:
	int price = 0;
public:
	AgilityPotion() {
		this->price = 150 + rand() % 251;
	}

	void drink(Player* player) override {
		player->setAgility(player->getAgility() + 100);
		cout << "I`m drink agility potion. Agility = " << player->getAgility() << endl;
	}

	int getPrice() {
		return this->price;
	}
};

class Engine {
private:
	string monsterName[10] = { "Kitana","Fearless Hannah","Cyborg killer","Bloodsucker","Scary Dracula","Roberto the shooter","Witcher","Your math teacher","russians","Volondemort" };
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
		string armorName[7] = { "Box", "Stone shield", "Iron shield", "Knight armour","Siver sheild","Ruby armor" };
		string name = armorName[random(0, 6)];
		return new Armor(armorInfo->getProtection(), name);
	}

	Weapon* generateWeapon(Weapon* weaponInfo) {
		string weaponName[7] = { "Fork","Knife","Ordinary sword","Silver sword","Ax","Fireballs","Crystal sword" };
		string name = weaponName[random(0, 6)];
		return new Weapon(weaponInfo->getDamage(), name);
	}


	Player* createPlayer(string name, int prof, int protection, int playerDamage) {
		return new Player(20 * info->setEndurance(prof), 30, 1, name, info->setPower(prof), info->setAgility(prof), info->setEndurance(prof));
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
	
	void useRandomSkill(HardHit* hardHit,OrdinaryHit* hit, Healing* heal, Player* player) {
		int ch = rand() % 3;
		if (ch == 1) {
			hardHit->use(player);
		}
		else if (ch == 2) {
			hit->use(player);
		}
		else {
			heal->use(player);
		}
	}
	
	void drinkPotion(bool checkHpPotion, bool checkEnPotion, bool checkAgPotion, HpPotion* hpPotion, EndurancePotion* enPotion, AgilityPotion* agPotion,Player* player) {
		int ch = 0;
		cout << "What potion do you want to drink? 1 - HpPotion, 2 - EndurancePotion, 3 - AgilityPotion.\n";
		cin >> ch;
		if (ch == 1) {
			if (checkHpPotion) {
				hpPotion->drink(player);
			}
			else {
				cout << "You do not have this potion, so can not drink it!\n";
			}

		}
		else if (ch == 2) {
			if (checkEnPotion) {
				enPotion->drink(player);
			}
			else {
				cout << "You do not have this potion, so can not drink it!\n";
			}
		}
		else {
			if (checkAgPotion) {
				agPotion->drink(player);
			}
			else {
				cout << "You do not have this potion, so can not drink it!\n";
			}
		}
	}

};



class Event {
private:
	Engine* engine = NULL;
	Player* info = NULL;
	Monster* monsterInfo = NULL;
	Weapon* weaponInfo = NULL;
	Armor* armorInfo = NULL;
	bool checkHpPotion = false;
	bool checkEnPotion = false;
	bool checkAgPotion = false;

	bool checkPlayerMoney(int cash, int price) {
		if (price <= cash) {
			return true;
		}

		return false;
	}
public:
	Event(Engine* engine, Player* player, Monster* mob, Weapon* weaponInfo, Armor* armorInfo) {
		this->engine = engine;
		this->info = player;
		this->monsterInfo = mob;
		this->weaponInfo = weaponInfo;
		this->armorInfo = armorInfo;
		this->checkHpPotion = false;
		this->checkEnPotion = false;
		this->checkAgPotion = false;
	}
	
	void shop(Player* player) {
		int answer;
		cout << "Welcome to the shop, where you can buy weapon-1 or armor-2. What do you want to buy?\n";
		cin >> answer;

		if (answer == 1) {
			vector<Weapon*> weaponList;

			for (int i = 0; i < 7; i++) {
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
				cout << "You do not have much money, so you can not buy it!\n";
			}
		}
		else if (answer==2) {
			vector<Armor*> armorList;

			for (int i = 0; i < 7; i++) {
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
				cout << "You do not have much money, so you can not buy it!\n";
			}
		}

		else {
			cout << "You made a mistake, please enter another number.\n";
		}
	}

	void meetingMonster() {
		cout << "You was going through the dark forest and you saw something... And sudenly the monster jumped out of leaves and said:\n";
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

	void potionShop(Player* player) {
		int answer = 0;
		HpPotion* hpPotion = new HpPotion;
		EndurancePotion* endurancePotion = new EndurancePotion;
		AgilityPotion* agilityPotion = new AgilityPotion;
		cout << "Welcome to the Potion Shop, where you can buy potions that can help you developing you characteristics!\n";
		cout << "What do you want to buy? 1 - HpPotion, 2 - EndurancePotion, 3 - AgilityPotion\n";
		cin >> answer;

		if (answer == 1) {
			if (this->checkPlayerMoney(player->getMoney(), hpPotion->getPrice())) {
				player->setHealth(player->getHealth());
				player->setMoney(player->getMoney() - hpPotion->getPrice());
				this->checkHpPotion = true;
			}
			else {
				cout << "You do not have much money, so you can not buy it!\n";
			}
		}
		else if (answer == 2) {
			if (this->checkPlayerMoney(player->getMoney(),endurancePotion->getPrice())) {
				player->setEndurance(player->getEndurance());
				player->setMoney(player->getMoney() - endurancePotion->getPrice());
				this->checkEnPotion = true;
			}
			else {
				cout << "You do not have much money, so you can not buy it!\n";
			}
		}
		else if (answer == 3) {
			if (this->checkPlayerMoney(player->getMoney(), agilityPotion->getPrice())) {
				player->setAgility(player->getAgility());
				player->setMoney(player->getMoney() - agilityPotion->getPrice());
				this->checkAgPotion = true;
			}
			else {
				cout << "You do not have much money, so you can not buy it!\n";
			}
		}
		else {
			cout << "You made a mistake, please enter another numer.\n";
		}
	}
	bool getCheckHpPotion() {
		return this->checkHpPotion;
	}
	bool getCheckEnPotion() {
		return this->checkEnPotion;
	}
	bool getCheckAgPotion() {
		return this->checkAgPotion;
	}
	void fight(Player* player, Monster* mob, Weapon* weaponInfo, Engine* engine, int prof) {
		int playerHealth = player->getHealth();
		int monsterHealth = mob->getHealth();
		int skillChoice = 0;
		OrdinaryHit* ordinaryHit = new OrdinaryHit();
		HardHit* hardHit = new HardHit();
		Healing* heal = new Healing();
		HpPotion* hpPotion = new HpPotion();
		EndurancePotion* enPotion = new EndurancePotion();
		AgilityPotion* agPotion = new AgilityPotion();
		cout << "3 2 1 FIGHT!\n";
		for (int i = 1; i < 5; i++) {
			cout << "You can choose a skill or drink a potion before you hit. What do you want to use? 1 - ordinary hit, 2 - random skill, 3 - drink potion.\n";
			cin >> skillChoice;
			if (skillChoice == 1) {
				ordinaryHit->use(player);
			}
			else if (skillChoice == 2) {
				engine->useRandomSkill(hardHit, ordinaryHit, heal, player);
			}
			else if (skillChoice == 3) {
				engine->drinkPotion(getCheckHpPotion(),getCheckEnPotion(),getCheckAgPotion(),hpPotion,enPotion,agPotion,player);
			}
			playerHealth -= mob->getDamage() + player->getHealing();
			monsterHealth -= player->setPlayerDamage(weaponInfo->getDamage(), prof);
			cout << i << "raund: " << " Your damage:" << player->setPlayerDamage(weaponInfo->getDamage(), prof) << " Your HP:" << playerHealth << endl;
			cout << "Monster damage: " << mob->getDamage() << " Monster HP: " << monsterHealth << endl;
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
	Engine* engine1 = NULL;
	Player* user = NULL;
	Monster* monster = NULL;
	Armor* armor = NULL;
	Weapon* weapon = NULL;
	Event* event = new Event(engine1,user,monster,weapon,armor);
	cout << "Enter your player name:\n";
	cin >> playerName;


	cout << "Welcome to my game, where you will need to fight with the monster. Good luck :) \n";
	cout << "Who you want to be? 1 - barbarian, 2 - tank, 3 - robber.\n";
	cin >> answer;

	if (answer > 3 || answer < 1) {
		cout << "You made a mistake, please enter another number.\n";
		return 0;
	}

	armor = engine->generateArmor(armor);
	weapon = engine->generateWeapon(weapon);
	user = engine->createPlayer(playerName, answer, user->setProtection(armor->getProtection(),answer), user->setPlayerDamage(weapon->getDamage(), answer));

	bool gameStatus = true;

	while (gameStatus){
		int chance = rand() % 100;
		int ch = rand()%3;
		
		if (chance<5) {
			event->shop(user);
			event->potionShop(user);
		}
		else if (chance > 6 && chance <=45) {
			Monster* monster = engine->generateMonster(user->getLevel());
			event->meetingMonster();
			event->fight(user, monster, weapon,engine, answer);
		}
		else if (chance > 46 && chance<=70) {
			if (ch == 1) {
				event->agilityUp();
			}
			else if (ch == 2) {
				event->enduranceUp();
			}
			else {
				event->powerUp();
			}
		}
		else {
			cout << "Nothing happend, but if you continue...\n";
		}
		cout << "Press any button to continue\n";
		cout << "Press 'q' to exit\n";
		char symbol = _getch();

		if (symbol == 'q') {
			gameStatus = false;
		}
	}
	return 1;
}
