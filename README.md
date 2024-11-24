# adventure_game

import java.util.Scanner;

// Temel Oyuncu Sınıfı
class Player {
    String name;
    String charName;
    int health;
    int damage;
    int money;

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
        this.health = health;
        this.money = money;
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

        Location safeHouse = new Location(player, "Güvenli Ev");
        BattleLocation forest = new BattleLocation(player, "Orman", new Enemy("Zombi", 10, 3, 4));

        while (true) {
            System.out.println("Lokasyon seçin:");
            System.out.println("1- Güvenli Ev");
            System.out.println("2- Orman");
            int locationChoice = input.nextInt();

            if (locationChoice == 1) {
                if (!safeHouse.onLocation()) break;
            } else if (locationChoice == 2) {
                if (!forest.onLocation()) break;
            } else {
                System.out.println("Geçersiz seçim.");
            }
        }
    }
}
