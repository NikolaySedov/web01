import json
import urllib.request


data = {
    'completed': False,
    'id': 1,
    'title': 'delectus aut autem',
    'userId': 1,
}

example_json_string = '{"completed": false, "id": 1, "title": "delectus aut autem" ,"userId" :1}';
print('example', example_json_string);
print('loads', json.loads(example_json_string))

json_string = json.dumps(data);
print('dumps', json_string);


with open('dump_file.json', 'w') as dump_file:
    json.dump(data, dump_file)

with open('load_file.json', 'r') as load_file:
    loaded_data = json.load(load_file)

print('load', loaded_data)

all_data = {
    'list': [ 'list1', 'list2', 'list3', ],
    'tuple': ( 'tuple1', 'tuple2', 'tuple3', ),
    'str': 'str',
    'int': 42,
    'float': 4.81516,
    'True': True,
    'False': False,
    'None': None,
}

print('indent', json.dumps(all_data, indent=4))
print('sort_keys', json.dumps(all_data, indent=4, sort_keys=True))
print('separators', json.dumps(all_data, indent=4, separators=(';', '---')))

all_data_json = json.dumps(all_data)

all_data_parsed = json.loads(all_data_json)

print('json 2 python', all_data_parsed)

class User():
    def __init__(self, data):
        self.id = data['id']
        self.name = data['name']
        self.username = data['username']
        self.email = data['email']
        self.address = data['address']
        self.phone = data['phone']
        self.website = data['website']
        self.company = data['company']
        self.todos_done = 0

    def get_city(self):
        try:
            return self.address['city']
        except:
            return None

    def get_geo(self):
        try:
            return self.address['geo']
        except:
            return None

    def get_dict(self):
        return {
            'id': self.id,
            'name': self.name,
            'username': self.username,
            'email': self.email,
            'address': self.address,
            'phone': self.phone,
            'website': self.website,
            'company': self.company,
            'todos_done': self.todos_done,
        }

def get_dist2(coords1, coords2):
    return (float(coords1['lat']) - float(coords2['lat'])) * (float(coords1['lat']) - float(coords2['lat'])) + (float(coords1['lng']) - float(coords2['lng'])) * (float(coords1['lng']) - float(coords2['lng']))

class Users():
    def __init__(self):
        self.users = []

    def fetch_data(self):
        try:
            with urllib.request.urlopen('https://jsonplaceholder.typicode.com/users') as response:
                data = json.loads(response.read().decode())
                for user in data:
                    self.users.append(User(user))
        except:
            print('Fetch users errror')

    def print_cities(self):
        cities = []
        for user in self.users:
            city = user.get_city()
            if city and not city in cities:
                cities.append(city)
        print(cities)

    def find_closest(self):
        min_dist = 1e12
        for user1 in self.users:
            for user2 in self.users:
                dist = get_dist2(user1.get_geo(), user2.get_geo())
                if user1.id != user2.id and dist < min_dist:
                    u1 = user1
                    u2 = user2
                    min_dist = dist
        return [ u1, u2, ]

    def dump(self, filename = 'users_dump.json'):
        data = []
        for user in self.users:
            data.append(user.get_dict())
        with open(filename, 'w') as dump_file:
            json.dump(data, dump_file)

    def fetch_todos(self):
        try:
            with urllib.request.urlopen('https://jsonplaceholder.typicode.com/todos') as response:
                data = json.loads(response.read().decode())
                for todo in data:
                    for user in self.users:
                        if user.id == todo['userId']:
                            user.todos_done += 1
        except:
            print('errror')



users = Users()

users.fetch_data()
users.print_cities()

closest_users = users.find_closest()
print(closest_users[0].address, closest_users[1].address)
users.dump()

users.fetch_todos()
users.dump('users_todos_dump.json')