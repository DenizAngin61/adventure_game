# adventure_game

import java.util.Scanner;

// Temel Oyuncu Sınıfı
class Player {
    String name;
    String charName;
    int health;
    int originalHealth;
    int damage;
    int money;
    Inventory inventory;

    public Player() {
        this.inventory = new Inventory();
    }

    public void selectChar() {
        Scanner input = new Scanner(System.in);
        System.out.println("Karakter seçin: ");
        System.out.println("1- Samuray (Hasar: 5, Sağlık: 21, Para: 15)");
        System.out.println("2- Okçu (Hasar: 7, Sağlık: 18, Para: 20)");
        System.out.println("3- Şövalye (Hasar: 8, Sağlık: 24, Para: 5)");
        int choice = input.nextInt();

        switch (choice) {
            case 1:
                setPlayer("Samuray", 5, 21, 15);
                break;
            case 2:
                setPlayer("Okçu", 7, 18, 20);
                break;
            case 3:
                setPlayer("Şövalye", 8, 24, 5);
                break;
            default:
                System.out.println("Geçersiz seçim, varsayılan olarak Samuray seçildi.");
                setPlayer("Samuray", 5, 21, 15);
        }
        System.out.println("Karakteriniz: " + this.charName);
    }

    private void setPlayer(String charName, int damage, int health, int money) {
        this.charName = charName;
        this.damage = damage;
        this.originalHealth = health;
        this.health = health;
        this.money = money;
    }

    public void printInfo() {
        System.out.println("Karakter: " + charName +
                "\nHasar: " + (damage + inventory.getWeaponDamage()) +
                "\nSağlık: " + health +
                "\nPara: " + money +
                "\nSilah: " + inventory.getWeapon() +
                "\nZırh: " + inventory.getArmor() +
                "\nEngelleme: " + inventory.getArmorDefence());
    }
}


//Envanter Sınıfı
class Inventory {
    private String weapon;
    private String armor;
    private int weaponDamage;
    private int armorDefence;

    public Inventory() {
        this.weapon = "Yok";
        this.armor = "Yok";
        this.weaponDamage = 0;
        this.armorDefence = 0;
    }

    public String getWeapon() {
        return weapon;
    }

    public void setWeapon(String weapon, int weaponDamage) {
        this.weapon = weapon;
        this.weaponDamage = weaponDamage;
    }

    public String getArmor() {
        return armor;
    }

    public void setArmor(String armor, int armorDefence) {
        this.armor = armor;
        this.armorDefence = armorDefence;
    }

    public int getWeaponDamage() {
        return weaponDamage;
    }

    public int getArmorDefence() {
        return armorDefence;
    }
}

//Dükkan Lokasyonu - Oyuncunun silah ve zırh alabileceği bir yer.

class ToolStore extends Location {

    public ToolStore(Player player) {
        super(player, "Mağaza");
    }

    @Override
    public boolean onLocation() {
        Scanner input = new Scanner(System.in);
        System.out.println("Mağazaya hoş geldiniz!");
        System.out.println("1- Silahlar");
        System.out.println("2- Zırhlar");
        System.out.println("3- Çıkış");
        int choice = input.nextInt();

        switch (choice) {
            case 1:
                printWeapons();
                buyWeapon();
                break;
            case 2:
                printArmors();
                buyArmor();
                break;
            case 3:
                System.out.println("Mağazadan çıkış yapılıyor...");
                return true;
            default:
                System.out.println("Geçersiz seçim.");
        }
        return true;
    }

    private void printWeapons() {
        System.out.println("Silahlar:");
        System.out.println("1- Tabanca (Hasar: 2, Fiyat: 15)");
        System.out.println("2- Kılıç (Hasar: 3, Fiyat: 25)");
        System.out.println("3- Tüfek (Hasar: 7, Fiyat: 45)");
    }

    private void buyWeapon() {
        Scanner input = new Scanner(System.in);
        int choice = input.nextInt();
        switch (choice) {
            case 1:
                if (player.money >= 15) {
                    player.money -= 15;
                    player.inventory.setWeapon("Tabanca", 2);
                    System.out.println("Tabanca satın alındı!");
                } else {
                    System.out.println("Yeterli paranız yok!");
                }
                break;
            case 2:
                if (player.money >= 25) {
                    player.money -= 25;
                    player.inventory.setWeapon("Kılıç", 3);
                    System.out.println("Kılıç satın alındı!");
                } else {
                    System.out.println("Yeterli paranız yok!");
                }
                break;
            case 3:
                if (player.money >= 45) {
                    player.money -= 45;
                    player.inventory.setWeapon("Tüfek", 7);
                    System.out.println("Tüfek satın alındı!");
                } else {
                    System.out.println("Yeterli paranız yok!");
                }
                break;
            default:
                System.out.println("Geçersiz seçim.");
        }
    }

    private void printArmors() {
        System.out.println("Zırhlar:");
        System.out.println("1- Hafif Zırh (Engelleme: 1, Fiyat: 15)");
        System.out.println("2- Orta Zırh (Engelleme: 3, Fiyat: 25)");
        System.out.println("3- Ağır Zırh (Engelleme: 5, Fiyat: 40)");
    }

    private void buyArmor() {
        Scanner input = new Scanner(System.in);
        int choice = input.nextInt();
        switch (choice) {
            case 1:
                if (player.money >= 15) {
                    player.money -= 15;
                    player.inventory.setArmor("Hafif Zırh", 1);
                    System.out.println("Hafif Zırh satın alındı!");
                } else {
                    System.out.println("Yeterli paranız yok!");
                }
                break;
            case 2:
                if (player.money >= 25) {
                    player.money -= 25;
                    player.inventory.setArmor("Orta Zırh", 3);
                    System.out.println("Orta Zırh satın alındı!");
                } else {
                    System.out.println("Yeterli paranız yok!");
                }
                break;
            case 3:
                if (player.money >= 40) {
                    player.money -= 40;
                    player.inventory.setArmor("Ağır Zırh", 5);
                    System.out.println("Ağır Zırh satın alındı!");
                } else {
                    System.out.println("Yeterli paranız yok!");
                }
                break;
            default:
                System.out.println("Geçersiz seçim.");
        }
    }
}

// Düşman Sınıfı
class Enemy {
    String name;
    int health;
    int damage;
    int reward;

    public Enemy(String name, int health, int damage, int reward) {
        this.name = name;
        this.health = health;
        this.damage = damage;
        this.reward = reward;
    }
}

// Mekan Sınıfı
class Location {
    Player player;
    String name;

    public Location(Player player, String name) {
        this.player = player;
        this.name = name;
    }

    public boolean onLocation() {
        return true;
    }
}

// Savaş Mekanı
class BattleLocation extends Location {
    Enemy enemy;

    public BattleLocation(Player player, String name, Enemy enemy) {
        super(player, name);
        this.enemy = enemy;
    }

    @Override
    public boolean onLocation() {
        System.out.println("Dikkat! " + enemy.name + " ile karşılaştınız!");
        while (player.health > 0 && enemy.health > 0) {
            System.out.println("Savaş başlıyor...");
            System.out.println(player.charName + " vuruyor!");
            enemy.health -= player.damage;
            if (enemy.health > 0) {
                System.out.println(enemy.name + " size saldırıyor!");
                player.health -= enemy.damage;
            }
        }

        if (player.health > 0) {
            System.out.println("Savaşı kazandınız! " + enemy.reward + " altın kazandınız.");
            player.money += enemy.reward;
            return true;
        } else {
            System.out.println("Savaşı kaybettiniz. Oyun bitti!");
            return false;
        }
    }
}

// Ana Oyun Sınıfı
public class AdventureGame {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.println("Macera Oyununa Hoş Geldiniz!");
        Player player = new Player();
        player.selectChar();

        Location[] locations = {
            new Location(player, "Güvenli Ev"),
            new ToolStore(player),
            new BattleLocation(player, "Orman", new Enemy("Zombi", 10, 3, 4))
        };

        while (true) {
            System.out.println("Lokasyon seçin:");
            for (int i = 0; i < locations.length; i++) {
                System.out.println((i + 1) + "- " + locations[i].name);
            }
            System.out.println((locations.length + 1) + "- Çıkış");

            int locationChoice = input.nextInt();
            if (locationChoice == locations.length + 1) {
                System.out.println("Oyun bitti!");
                break;
            }

            if (!locations[locationChoice - 1].onLocation()) break;
        }
    }
}

