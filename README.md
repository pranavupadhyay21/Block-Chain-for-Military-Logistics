# Block-Chain-for-Military-Logistics:
Language Used :
The following programming languages and tools are used:

●	Solidity : Used for writing smart contracts on Ethereum blockchain.

●	Python : Employed for scripting and automation tasks.

●	JavaScript : Utilized for developing web interfaces and interacting with the blockchain.

●	Hyperledger Fabric : A permissioned blockchain framework, chosen for its modular architecture and suitability for enterprise applications.

Military logistics is a complex and critical aspect of national defense operations, involving the planning, implementation, and coordination of the movement and maintenance of forces. Ensuring the accuracy, efficiency, and security of these logistics operations is paramount. Blockchain technology, with its decentralized and immutable ledger system, presents a promising solution to the challenges faced in military logistics.
The primary objective of this project is to investigate and implement blockchain technology in military logistics to:
●	 Improve the accuracy and transparency of supply chain operations.

●	 Enhance the security of logistical data and communications.

●	 Automate and streamline procurement and supply processes using smart contracts.

●	 Ensure real-time tracking and traceability of military supplies.


1.3 Motivation :
●	Data Integrity : Traditional logistics systems are prone to errors and data manipulation. Blockchain's immutable ledger ensures that once data is recorded, it cannot be altered, thus maintaining data integrity.
●	Supply Chain Visibility : A transparent supply chain is crucial for effective logistics management. Blockchain provides end-to-end visibility, allowing for real-time tracking of supplies and enhancing accountability.

●	Cybersecurity : Military logistics systems are frequent targets of cyber-attacks. Blockchain's decentralized and encrypted nature provides robust security against unauthorized access and tampering.

●	Operational Efficiency : Manual processes in logistics are time-consuming and error-prone. Smart contracts can automate various logistical operations, reducing human error and speeding up the process.

Code: 
import hashlib
import json
import time
from flask import Flask, render_template, jsonify

class User:
    def __init__(self, username, password, role):
        self.username = username
        self.password = hashlib.sha256(password.encode()).hexdigest()
        self.role = role

class Blockchain:
    def __init__(self):
        self.chain = []
        self.create_block(proof=1, previous_hash='0')

    def create_block(self, proof, previous_hash):
        block = {
            'index': len(self.chain) + 1,
            'timestamp': time.time(),
            'proof': proof,
            'previous_hash': previous_hash,
            # Add more data as needed
        }
        self.chain.append(block)
        return block

    def get_previous_block(self):
        return self.chain[-1]

    def proof_of_work(self, previous_proof):
        new_proof = 1
        check_proof = False
        while not check_proof:
            hash_operation = hashlib.sha256(str(new_proof**2 - previous_proof**2).encode()).hexdigest()
            if hash_operation[:4] == '0000':
                check_proof = True
            else:
                new_proof += 1
        return new_proof

    def hash(self, block):
        encoded_block = json.dumps(block, sort_keys=True).encode()
        return hashlib.sha256(encoded_block).hexdigest()

    def is_chain_valid(self, chain):
        previous_block = chain[0]
        block_index = 1
        while block_index < len(chain):
            block = chain[block_index]
            if block['previous_hash'] != self.hash(previous_block):
                return False
            previous_proof = previous_block['proof']
            proof = block['proof']
            hash_operation = hashlib.sha256(str(proof**2 - previous_proof**2).encode()).hexdigest()
            if hash_operation[:4] != '0000':
                return False
            previous_block = block
            block_index += 1
        return True

class MilitaryLogisticsSystem:
    def __init__(self):
        self.users = []
        self.blockchain = Blockchain()
        self.app = Flask(__name__)
        self.register_routes()
        self.register_admin()
        self.login_prompt()

    def register_routes(self):
        @self.app.route('/')
        def home():
            warehouse_locations = [
                {"name": "Warehouse Mumbai", "lat": 19.0760, "lng": 72.8777, "content": "This is Warehouse Mumbai"},  # Mumbai
                {"name": "Warehouse Shimla", "lat": 31.1048, "lng": 77.1734, "content": "This is Warehouse Shimla"},  # Shimla
                {"name": "Warehouse Bhopal", "lat": 23.2599, "lng": 77.4126, "content": "This is Warehouse Bhopal"},  # Bhopal
                {"name": "Warehouse Tamil Nadu", "lat": 11.1271, "lng": 78.6569, "content": "This is Warehouse Tamil Nadu"},  # Tamil Nadu
                {"name": "Warehouse Assam", "lat": 26.2006, "lng": 92.9376, "content": "This is Warehouse Assam"}   # Assam
            ]
            return render_template('warehouses.html', locations=warehouse_locations)

    def register_admin(self):
        print("=== Admin Registration ===")
        username = input("Enter admin username: ")
        password = input("Enter admin password: ")
        admin = User(username, password, 'admin')
        self.users.append(admin)
        print("Admin registered successfully!")

    def register_user(self):
        print("\n=== User Registration ===")
        username = input("Enter username: ")
        password = input("Enter password: ")
        user = User(username, password, 'user')
        self.users.append(user)
        print("User registered successfully!")

    def login(self):
        username = input("Enter username: ")
        password = hashlib.sha256(input("Enter password: ").encode()).hexdigest()
        for user in self.users:
            if user.username == username and user.password == password:
                print("Login successful!")
                return user
        print("Invalid username or password!")
        return None

    def login_prompt(self):
        print("\n=== Login ===")
        while True:
            user = self.login()
            if user and user.role == 'admin':
                self.display_admin_menu(user)
                break

    def display_main_menu(self, admin):
        print("\nMain Menu:")
        print("1. Add User")
        print("2. Warehouse")
        print("3. Inventory")
        print("4. ATM Card Details")
        print("5. Check Registered Users")
        print("6. Logout")

    def display_admin_menu(self, admin):
        print("Welcome, Admin", admin.username)
        while True:
            self.display_main_menu(admin)
            choice = input("Enter your choice: ")
            if choice == '1':
                self.register_user()
            elif choice == '2':
                self.show_warehouse_locations()
            elif choice == '3':
                self.inventory_menu()
            elif choice == '4':
                self.atm_card_menu(admin)
            elif choice == '5':
                self.check_registered_users(admin)
            elif choice == '6':
                print("Logging out...")
                break
            else:
                print("Invalid choice!")

        def inventory_menu(self):
         print("\nInventory Menu:")
         print("1. Add Guns")
         print("2. Add Missiles")
         print("3. Add Grenades")
         print("4. Add Tanks")
         print("5. Add Armored Vehicles")
         print("6. Add Jets")
         print("7. Add Submarines")
         print("8. Add Nuclear Weapons")
         print("9. View Inventory")
         choice = input("Enter your choice: ")
         if choice == '1':
             self.add_guns()
         elif choice == '2':
             self.add_missiles()
         elif choice == '3':
             self.add_grenades()
         elif choice == '4':
             self.add_tanks()
         elif choice == '5':
             self.add_armored_vehicles()
         elif choice == '6':
             self.add_jets()
         elif choice == '7':
             self.add_submarines()
         elif choice == '8':
             self.add_nuclear_weapons()
         elif choice == '9':
             self.view_inventory()
         else:
             print("Invalid choice!")
 
    def add_guns(self):
        print("\nAdding Guns...")
        # Here you can add the implementation to add guns to the inventory
        # For example:
        # gun_name = input("Enter gun name: ")
        # quantity = int(input("Enter quantity: "))
        # Add the guns to the inventory storage

    def add_missiles(self):
        print("\nAdding Missiles...")
        # Implement adding missiles to the inventory

    def add_grenades(self):
        print("\nAdding Grenades...")
        # Implement adding grenades to the inventory

    def add_tanks(self):
        print("\nAdding Tanks...")
        # Implement adding tanks to the inventory

    def add_armored_vehicles(self):
        print("\nAdding Armored Vehicles...")
        # Implement adding armored vehicles to the inventory

    def add_jets(self):
        print("\nAdding Jets...")
        # Implement adding jets to the inventory

    def add_submarines(self):
        print("\nAdding Submarines...")
        # Implement adding submarines to the inventory

    def add_nuclear_weapons(self):
        print("\nAdding Nuclear Weapons...")
        # Implement adding nuclear weapons to the inventory

    def view_inventory(self):
        print("\nView Inventory:")
        # Implement viewing the inventory

    def atm_card_menu(self, admin):
        print("\nATM Card Menu:")
        print("1. Add Card Details")
        print("2. Check Card Details")
        choice = input("Enter your choice: ")
        if choice == '1':
            self.add_card_details(admin)
        elif choice == '2':
            self.check_card_details(admin)
        else:
            print("Invalid choice!")

    def add_card_details(self, admin):
        card_number = input("Enter card number: ")
        expiry_date = input("Enter expiry date (MM/YYYY): ")
        cvv = input("Enter CVV: ")
        print("Card details added successfully!")

    def check_card_details(self, admin):
        print("Card Number: 1234 XXXX XXXX 5678")
        print("Expiry Date: 12/2023")
        print("CVV: XXX")

    def check_registered_users(self, admin):
        print("\nRegistered Users:")
        for user in self.users:
            if user.role == 'user':
                print(user.username)

    def show_warehouse_locations(self):
        print("Open the following URL in a web browser to see the warehouse locations:")
        print("http://127.0.0.1:5500/final/test_1.html")

    def run_server(self):
        self.app.run(debug=True)

# Start the logistics system
if __name__ == '__main__':
    logistics_system = MilitaryLogisticsSystem()
    logistics_system.run_server()

HTML CODE:


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Warehouse Locations</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
    <style>
        /* Set the size of the map div */
        #map {
            height: 100vh; /* Full viewport height */
            width: 100%;   /* Full width */
        }
        /* Optional: Make the page full-height */
        html, body {
            height: 100%;
            margin: 0;
            padding: 0;
        }
    </style>
</head>
<body>
    <h1>Warehouse Locations</h1>
    <div id="map"></div>
    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
    <script>
        // Define the warehouse locations
        var locations = [
            {lat: 19.0760, lng: 72.8777, name: 'Warehouse Mumbai', content: 'This is Warehouse Mumbai'},  // Mumbai
            {lat: 31.1048, lng: 77.1734, name: 'Warehouse Shimla', content: 'This is Warehouse Shimla'},  // Shimla
            {lat: 23.2599, lng: 77.4126, name: 'Warehouse Bhopal', content: 'This is Warehouse Bhopal'}   // Bhopal
        ];

        // Initialize the map and set its view to the center of India
        var map = L.map('map').setView([22.3511, 78.6677], 6);

        // Add OpenStreetMap tiles
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '© OpenStreetMap contributors'
        }).addTo(map);

        // Add markers for each location
        locations.forEach(function(location) {
            var marker = L.marker([location.lat, location.lng]).addTo(map)
                .bindPopup(`<b>${location.name}</b><br>${location.content}`);
        });
    </script>
</body>
</html>
