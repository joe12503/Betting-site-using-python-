# Betting-site-using-python
class BettingSite:
    def __init__(self):
        self.users = {}
        self.events = {}

    def register_user(self, username):
        if username not in self.users:
            self.users[username] = 1000  # Initial balance for new users
            return True
        else:
            return False  # User already exists

    def create_event(self, event_name):
        if event_name not in self.events:
            self.events[event_name] = {'participants': {}, 'bets': {}}
            return True
        else:
            return False  # Event already exists

    def add_participant(self, event_name, participant_name):
        if event_name in self.events and participant_name not in self.events[event_name]['participants']:
            self.events[event_name]['participants'][participant_name] = 0
            return True
        else:
            return False  # Participant or event not found

    def place_bet(self, username, event_name, participant_name, amount):
        if username in self.users and event_name in self.events and participant_name in self.events[event_name]['participants']:
            if amount > 0 and amount <= self.users[username]:
                self.users[username] -= amount
                self.events[event_name]['participants'][participant_name] += amount
                self.events[event_name]['bets'][username] = {'participant': participant_name, 'amount': amount}
                return True
        return False  # Bet placement unsuccessful

    def resolve_event(self, event_name, winning_participant):
        if event_name in self.events and winning_participant in self.events[event_name]['participants']:
            for username, bet_info in self.events[event_name]['bets'].items():
                if bet_info['participant'] == winning_participant:
                    winnings = 2 * bet_info['amount']  # Simple 2x payout for simplicity
                    self.users[username] += winnings
            self.events.pop(event_name)
            return True
        else:
            return False  # Event or winning participant not found


# Example Usage
betting_site = BettingSite()

betting_site.register_user("user1")
betting_site.register_user("user2")

betting_site.create_event("football_match")
betting_site.add_participant("football_match", "team1")
betting_site.add_participant("football_match", "team2")

betting_site.place_bet("user1", "football_match", "team1", 50)
betting_site.place_bet("user2", "football_match", "team2", 30)

betting_site.resolve_event("football_match", "team1")

print("User1 balance:", betting_site.users["user1"])
print("User2 balance:", betting_site.users["user2"])
```
