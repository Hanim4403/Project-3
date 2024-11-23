import calendar

class Caregiver:
    def __init__(self, year, month):
        if not (1 <= month <= 12):
            raise ValueError("Month must be between 1 and 12")
        self.year = year
        self.month = month
        self.time = ["7:00AM - 1:00PM", "1:00PM - 7:00PM"]

    def days_schedule(self):
        try:
            num_days = calendar.monthrange(self.year, self.month)[1]
            start_day = calendar.monthrange(self.year, self.month)[0]  
        except IndexError as e:
            raise ValueError(f"Invalid year or month: {self.year}-{self.month}") from e

        schedule = {}
        am = ["Hani M", "Ria P", "Amr K"]
        pm = ["Amr K", "Hani M", "Ria P"]
        days_of_week = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]

        for day in range(1, num_days + 1):
            day_of_week = days_of_week[(start_day + (day - 1)) % 7]
            schedule[f"{self.month}/{day}/{self.year} ({day_of_week})"] = {
                self.time[0]: am[(day - 1) % len(am)],
                self.time[1]: pm[(day - 1) % len(pm)]
            }
        return schedule

    def create(self):
        schedule = self.days_schedule()
        for date, time in schedule.items():
            print(f"{date}: {time['7:00AM - 1:00PM']} will work AM shift")
            print(f"{date}: {time['1:00PM - 7:00PM']} will work PM shift")

    def generate_html_calendar(self):
        schedule = self.days_schedule()
        html_content = "<html><head><title>Caregiver Schedule</title></head><body>"
        html_content += "<h1>Caregiver Schedule</h1>"
        html_content += "<table border='1'><tr><th>Date</th><th>AM Caregiver (7:00AM - 1:00PM)</th><th>PM Caregiver (1:00PM - 7:00PM)</th></tr>"
        for date, time in schedule.items():
            html_content += f"<tr><td>{date}</td><td>{time[self.time[0]]}</td><td>{time[self.time[1]]}</td></tr>"
        html_content += "</table></body></html>"
        with open("caregiver_schedule.html", "w") as file:
            file.write(html_content)

if __name__ == "__main__":
    caregiver = Caregiver(2024, 11)
    caregiver.create()
    caregiver.generate_html_calendar()
