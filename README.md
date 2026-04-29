#!/usr/bin/env python3
"""
=============================================================================
 🕐  Python datetime & pytz — Comprehensive Practice Script (with Solutions)
=============================================================================

Sections:
  1. Basics         — date, time, datetime objects
  2. Formatting     — strftime (datetime → string)
  3. Parsing        — strptime (string → datetime)
  4. Timezones      — pytz and timezone-aware datetimes
  5. Timedeltas     — date/time arithmetic
  6. Real-World     — putting it all together

Each section has explanations, runnable examples, and ✅ SOLUTIONS.
"""

# ===========================================================================
# SECTION 1 — BASICS: date, time, and datetime
# ===========================================================================
print("=" * 70)
print("  SECTION 1 — BASICS")
print("=" * 70, "\n")

from datetime import date, time, datetime

# --- date objects ---
today = date.today()
print("Today's date :", today)
print("Year         :", today.year)
print("Month        :", today.month)
print("Day          :", today.day)
print("Day of week  :", today.weekday(), " (0=Mon, 6=Sun)")
print("ISO weekday  :", today.isoweekday(), " (1=Mon, 7=Sun)")
print()

# --- time objects ---
t = time(14, 30, 45)
print("Time         :", t)
print("Hour         :", t.hour)
print("Minute       :", t.minute)
print("Second       :", t.second)
print()

# --- datetime objects ---
now = datetime.now()
print("Now          :", now)
print("Date part    :", now.date())
print("Time part    :", now.time())
print("Timestamp    :", now.timestamp())

launch = datetime(2026, 7, 4, 9, 0, 0)
print("Launch       :", launch)
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 1.1 — Birthday day of week
# ---------------------------------------------------------------------------
print("--- Exercise 1.1: Birthday Day of Week ---")

# ✅ SOLUTION (example: August 15)
my_birthday = date(2026, 8, 15)
day_name = my_birthday.strftime("%A")
print(f"My birthday ({my_birthday}) falls on a {day_name}")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 1.2 — New Year's Eve datetime
# ---------------------------------------------------------------------------
print("--- Exercise 1.2: New Year's Eve ---")

# ✅ SOLUTION
nye = datetime(2026, 12, 31, 23, 59, 59)
print("Full datetime:", nye)
print("Date part    :", nye.date())
print("Time part    :", nye.time())
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 1.3 — Extract current time components
# ---------------------------------------------------------------------------
print("--- Exercise 1.3: Time Components ---")

# ✅ SOLUTION
now = datetime.now()
print("Current hour       :", now.hour)
print("Current minute     :", now.minute)
print("Current microsecond:", now.microsecond)
print()


# ===========================================================================
# SECTION 2 — FORMATTING: strftime (datetime → string)
# ===========================================================================
print("=" * 70)
print("  SECTION 2 — FORMATTING (strftime)")
print("=" * 70, "\n")

"""
Common Format Codes:
  %Y  4-digit year          2026        %A  Full weekday       Wednesday
  %y  2-digit year          26          %a  Abbreviated weekday Wed
  %m  Month (zero-padded)   04          %H  Hour 24h           14
  %B  Full month name       April       %I  Hour 12h           02
  %b  Abbreviated month     Apr         %M  Minute             07
  %d  Day (zero-padded)     29          %S  Second             00
  %p  AM/PM                 PM          %Z  Timezone name      EDT
  %j  Day of year           119         %W  Week number        17
"""

now = datetime.now()
print("ISO format      :", now.strftime("%Y-%m-%d %H:%M:%S"))
print("US style        :", now.strftime("%m/%d/%Y"))
print("European style  :", now.strftime("%d/%m/%Y"))
print("Human-friendly  :", now.strftime("%A, %B %d, %Y at %I:%M %p"))
print("Compact         :", now.strftime("%Y%m%d_%H%M%S"))
print("Log format      :", now.strftime("[%Y-%m-%d %H:%M:%S]"))
print("Day of year     :", now.strftime("Day %j of %Y"))
print()
print("isoformat()     :", now.isoformat())
print("date isoformat  :", now.date().isoformat())
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 2.1 — Custom format: "Wed, Apr 29 2026 - 02:07 PM"
# ---------------------------------------------------------------------------
print("--- Exercise 2.1: Custom Format ---")

# ✅ SOLUTION
formatted = datetime.now().strftime("%a, %b %d %Y - %I:%M %p")
print(formatted)
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 2.2 — Multiple formats for July 4
# ---------------------------------------------------------------------------
print("--- Exercise 2.2: Multiple Formats ---")

# ✅ SOLUTION
july4 = datetime(2026, 7, 4, 9, 0, 0)
print("a)", july4.strftime("%Y-%m-%d"))
print("b)", july4.strftime("%m/%d/%y"))
print("c)", july4.strftime("%A, %B %d, %Y %I:%M:%S %p"))
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 2.3 — Receipt date function
# ---------------------------------------------------------------------------
print("--- Exercise 2.3: Receipt Date Function ---")

# ✅ SOLUTION
def format_receipt_date(dt):
    return dt.strftime("%m/%d/%Y %I:%M %p")

print(format_receipt_date(datetime.now()))
print()


# ===========================================================================
# SECTION 3 — PARSING: strptime (string → datetime)
# ===========================================================================
print("=" * 70)
print("  SECTION 3 — PARSING (strptime)")
print("=" * 70, "\n")

# strptime("string", "format") → datetime
# ⚠️ The format MUST match the string exactly or you get ValueError.

d1 = datetime.strptime("2026-04-29", "%Y-%m-%d")
print("Parsed ISO      :", d1)

d2 = datetime.strptime("04/29/2026", "%m/%d/%Y")
print("Parsed US       :", d2)

d3 = datetime.strptime("29-Apr-2026 14:30", "%d-%b-%Y %H:%M")
print("Parsed custom   :", d3)

d4 = datetime.strptime("Wednesday, April 29, 2026", "%A, %B %d, %Y")
print("Parsed long     :", d4)

d5 = datetime.strptime("2026-04-29T14:07:00", "%Y-%m-%dT%H:%M:%S")
print("Parsed ISO 8601 :", d5)
print()

# Common mistake & fix
print("--- Common Mistake ---")
try:
    bad = datetime.strptime("04-29-2026", "%Y-%m-%d")  # Wrong order!
except ValueError as e:
    print("ValueError:", e)
good = datetime.strptime("04-29-2026", "%m-%d-%Y")
print("Fixed:", good)
print()

# Parse then reformat
raw = "April 29, 2026 2:07 PM"
parsed = datetime.strptime(raw, "%B %d, %Y %I:%M %p")
reformatted = parsed.strftime("%Y-%m-%d %H:%M")
print(f"'{raw}'  →  '{reformatted}'")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 3.1 — Parse DD/MM/YYYY and get weekday
# ---------------------------------------------------------------------------
print("--- Exercise 3.1: Parse DD/MM/YYYY ---")

# ✅ SOLUTION
dt = datetime.strptime("15/06/2026", "%d/%m/%Y")
print(f"Parsed: {dt}")
print(f"Day of week: {dt.strftime('%A')}")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 3.2 — Parse and reformat
# ---------------------------------------------------------------------------
print("--- Exercise 3.2: Parse and Reformat ---")

# ✅ SOLUTION
raw = "Sat, 04 Jul 2026 09:00:00"
parsed = datetime.strptime(raw, "%a, %d %b %Y %H:%M:%S")
reformatted = parsed.strftime("%B %d, %Y at %I:%M %p")
print(f"'{raw}'  →  '{reformatted}'")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 3.3 — Flexible parser
# ---------------------------------------------------------------------------
print("--- Exercise 3.3: Flexible Parser ---")

# ✅ SOLUTION
def parse_flexible(date_str):
    formats = ["%Y-%m-%d", "%m/%d/%Y", "%d-%b-%Y", "%B %d, %Y"]
    for fmt in formats:
        try:
            return datetime.strptime(date_str, fmt)
        except ValueError:
            continue
    raise ValueError(f"Could not parse '{date_str}' with any known format")

test_dates = ["2026-04-29", "04/29/2026", "29-Apr-2026", "April 29, 2026"]
for d in test_dates:
    print(f"  '{d}' → {parse_flexible(d)}")
print()


# ===========================================================================
# SECTION 4 — TIMEZONES: pytz and timezone-aware datetimes
# ===========================================================================
print("=" * 70)
print("  SECTION 4 — TIMEZONES (pytz)")
print("=" * 70, "\n")

"""
Naive vs. Aware:
  Naive  — NO timezone info (tzinfo is None)
  Aware  — HAS timezone info attached

pytz gives named timezones with proper DST handling.
Python 3.9+ also has zoneinfo (standard library).
"""

import pytz

print(f"Total pytz timezones: {len(pytz.all_timezones)}")
print("\nCommon US timezones:")
for tz in sorted(pytz.all_timezones):
    if tz.startswith("US/"):
        print(f"  {tz}")
print()

# Creating timezone-aware datetimes
eastern = pytz.timezone("US/Eastern")
naive_dt = datetime(2026, 4, 29, 14, 0, 0)
aware_dt = eastern.localize(naive_dt)           # ✅ CORRECT with pytz
print("Localized   :", aware_dt)
print("Timezone    :", aware_dt.tzinfo)
print("UTC offset  :", aware_dt.strftime("%z"))
print()

# Current time in a specific timezone
tokyo_tz = pytz.timezone("Asia/Tokyo")
tokyo_now = datetime.now(tokyo_tz)
print("Tokyo now   :", tokyo_now)
print()

# ⚠️  WRONG vs RIGHT way
print("--- replace() vs localize() ---")
naive = datetime(2026, 7, 4, 12, 0, 0)
wrong = naive.replace(tzinfo=eastern)    # ❌ May give wrong offset (LMT)
right = eastern.localize(naive)          # ✅ Correct
print(f"replace() [WRONG] : {wrong} → offset: {wrong.strftime('%z')}")
print(f"localize() [RIGHT]: {right} → offset: {right.strftime('%z')}")
print()

# Converting between timezones
pacific = pytz.timezone("US/Pacific")
london = pytz.timezone("Europe/London")
tokyo = pytz.timezone("Asia/Tokyo")

dt_east = eastern.localize(datetime(2026, 4, 29, 14, 0, 0))
print("Converting 2:00 PM Eastern to other timezones:")
print("  Eastern  :", dt_east.strftime("%Y-%m-%d %I:%M %p %Z"))
print("  Pacific  :", dt_east.astimezone(pacific).strftime("%Y-%m-%d %I:%M %p %Z"))
print("  London   :", dt_east.astimezone(london).strftime("%Y-%m-%d %I:%M %p %Z"))
print("  Tokyo    :", dt_east.astimezone(tokyo).strftime("%Y-%m-%d %I:%M %p %Z"))
print("  UTC      :", dt_east.astimezone(pytz.utc).strftime("%Y-%m-%d %I:%M %p %Z"))
print()

# DST awareness
print("--- DST Awareness ---")
summer = eastern.localize(datetime(2026, 7, 1, 12, 0))   # EDT
winter = eastern.localize(datetime(2026, 12, 1, 12, 0))  # EST
print(f"Summer: {summer.strftime('%Z')} (UTC{summer.strftime('%z')})")
print(f"Winter: {winter.strftime('%Z')} (UTC{winter.strftime('%z')})")
print()

# BONUS: zoneinfo (Python 3.9+)
print("--- BONUS: zoneinfo (Python 3.9+) ---")
from zoneinfo import ZoneInfo
dt_zi = datetime(2026, 4, 29, 14, 0, tzinfo=ZoneInfo("US/Eastern"))
print("zoneinfo     :", dt_zi)
print("Convert to PT:", dt_zi.astimezone(ZoneInfo("US/Pacific")))
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 4.1 — Time in other cities
# ---------------------------------------------------------------------------
print("--- Exercise 4.1: World Times ---")

# ✅ SOLUTION
dt_ny = eastern.localize(datetime(2026, 4, 29, 15, 0, 0))
cities = {
    "Los Angeles": "US/Pacific",
    "London":      "Europe/London",
    "Sydney":      "Australia/Sydney",
}
print(f"New York    : {dt_ny.strftime('%I:%M %p %Z')}")
for city, tz_name in cities.items():
    tz = pytz.timezone(tz_name)
    local = dt_ny.astimezone(tz)
    print(f"{city:<12}: {local.strftime('%I:%M %p %Z')}")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 4.2 — World clock function
# ---------------------------------------------------------------------------
print("--- Exercise 4.2: World Clock Function ---")

# ✅ SOLUTION
def world_clock(dt_aware):
    zones = {
        "New York": "US/Eastern",
        "London":   "Europe/London",
        "Dubai":    "Asia/Dubai",
        "Tokyo":    "Asia/Tokyo",
        "Sydney":   "Australia/Sydney",
    }
    for city, tz_name in zones.items():
        tz = pytz.timezone(tz_name)
        local = dt_aware.astimezone(tz)
        print(f"  {city:<12}: {local.strftime('%I:%M %p %Z')}")

now_eastern = datetime.now(pytz.timezone("US/Eastern"))
world_clock(now_eastern)
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 4.3 — Flight duration (JFK → Heathrow)
# ---------------------------------------------------------------------------
print("--- Exercise 4.3: Flight Duration ---")

# ✅ SOLUTION
london_tz = pytz.timezone("Europe/London")
depart = eastern.localize(datetime(2026, 4, 30, 20, 0, 0))
arrive = london_tz.localize(datetime(2026, 5, 1, 8, 0, 0))
depart_utc = depart.astimezone(pytz.utc)
arrive_utc = arrive.astimezone(pytz.utc)
flight_time = arrive_utc - depart_utc
total_hours = flight_time.total_seconds() / 3600

print(f"Depart JFK  : {depart.strftime('%I:%M %p %Z')} ({depart_utc.strftime('%H:%M UTC')})")
print(f"Arrive LHR  : {arrive.strftime('%I:%M %p %Z')} ({arrive_utc.strftime('%H:%M UTC')})")
print(f"Flight time : {flight_time} ({total_hours:.1f} hours)")
print()


# ===========================================================================
# SECTION 5 — TIMEDELTAS: Date & Time Arithmetic
# ===========================================================================
print("=" * 70)
print("  SECTION 5 — TIMEDELTAS")
print("=" * 70, "\n")

from datetime import timedelta

# Creating timedeltas
print("1 day      :", timedelta(days=1))
print("1 week     :", timedelta(weeks=1))
print("2 hours    :", timedelta(hours=2))
mixed = timedelta(days=3, hours=5, minutes=30)
print("Mixed      :", mixed)
print("Total secs :", mixed.total_seconds())
print()

# Adding & subtracting
now = datetime.now()
print("Now            :", now.strftime("%Y-%m-%d %H:%M"))
print("+ 7 days       :", (now + timedelta(days=7)).strftime("%Y-%m-%d %H:%M"))
print("- 30 days      :", (now - timedelta(days=30)).strftime("%Y-%m-%d %H:%M"))
print("+ 2h 30m       :", (now + timedelta(hours=2, minutes=30)).strftime("%Y-%m-%d %H:%M"))
print("+ 1 week       :", (now + timedelta(weeks=1)).strftime("%Y-%m-%d %H:%M"))
print()

# Countdown
new_year = datetime(2027, 1, 1)
countdown = new_year - now
total_secs = int(countdown.total_seconds())
days = total_secs // 86400
hours = (total_secs % 86400) // 3600
minutes = (total_secs % 3600) // 60
print(f"Days until 2027 : {countdown.days}")
print(f"Countdown       : {days}d {hours}h {minutes}m")
print()

# Comparing datetimes
deadline = datetime(2026, 5, 15, 17, 0, 0)
if datetime.now() < deadline:
    print(f"✅ {(deadline - datetime.now()).days} days until deadline.")
else:
    print(f"❌ Deadline passed {(datetime.now() - deadline).days} days ago!")
print()

# Generating a date series
start = datetime(2026, 5, 1)
print("Weekly meetings in May 2026:")
for i in range(5):
    meeting = start + timedelta(weeks=i)
    print(f"  Meeting {i+1}: {meeting.strftime('%A, %B %d')}")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 5.1 — Age in days
# ---------------------------------------------------------------------------
print("--- Exercise 5.1: Age in Days ---")

# ✅ SOLUTION (example birthdate — change to yours!)
birthdate = datetime(1995, 8, 15)
age_days = (datetime.now() - birthdate).days
print(f"Born: {birthdate.date()}")
print(f"Age in days: {age_days:,} (~{age_days / 365.25:.1f} years)")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 5.2 — Business days from now
# ---------------------------------------------------------------------------
print("--- Exercise 5.2: Business Days ---")

# ✅ SOLUTION
def business_days_from_now(n):
    current = datetime.now()
    added = 0
    while added < n:
        current += timedelta(days=1)
        if current.weekday() < 5:  # Mon=0 ... Fri=4
            added += 1
    return current.date()

print("10 business days from now:", business_days_from_now(10))
print("20 business days from now:", business_days_from_now(20))
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 5.3 — Subscription renewals
# ---------------------------------------------------------------------------
print("--- Exercise 5.3: Subscription Renewals ---")

# ✅ SOLUTION
sub_start = datetime(2026, 1, 15)
print(f"Start: {sub_start.date()}")
for i in range(1, 7):
    renewal = sub_start + timedelta(days=30 * i)
    print(f"  Renewal {i}: {renewal.strftime('%B %d, %Y (%A)')}")
print()

# ---------------------------------------------------------------------------
# 📝 Exercise 5.4 — Human-friendly countdown
# ---------------------------------------------------------------------------
print("--- Exercise 5.4: Countdown Function ---")

# ✅ SOLUTION
def time_until(event_name, event_dt):
    delta = event_dt - datetime.now()
    if delta.total_seconds() < 0:
        print(f"  {event_name} has already passed!")
        return
    ts = int(delta.total_seconds())
    d, h, m = ts // 86400, (ts % 86400) // 3600, (ts % 3600) // 60
    print(f"  {event_name} is in {d} days, {h} hours, {m} minutes")

time_until("🎆 New Year's Eve", datetime(2026, 12, 31, 23, 59, 59))
time_until("☀️  Summer Solstice", datetime(2026, 6, 20, 22, 32))
print()


# ===========================================================================
# SECTION 6 — REAL-WORLD SCENARIOS
# ===========================================================================
print("=" * 70)
print("  SECTION 6 — REAL-WORLD SCENARIOS")
print("=" * 70, "\n")

# ---------------------------------------------------------------------------
# 6A — Log Timestamp Parser
# ---------------------------------------------------------------------------
print("--- 6A: Log Timestamp Parser ---\n")

logs = [
    "[2026-04-29 08:15:23] INFO  Server started",
    "[2026-04-29 08:15:24] INFO  Database connected",
    "[2026-04-29 09:42:17] WARN  High memory usage detected",
    "[2026-04-29 09:43:01] ERROR Out of memory exception",
    "[2026-04-29 09:43:02] INFO  Auto-restart triggered",
    "[2026-04-29 09:43:08] INFO  Server started",
    "[2026-04-29 14:01:55] ERROR Connection timeout to API",
    "[2026-04-29 14:02:10] INFO  Retry succeeded",
]

events = []
for log in logs:
    ts = datetime.strptime(log[1:20], "%Y-%m-%d %H:%M:%S")
    level = log[22:27].strip()
    message = log[28:]
    events.append((ts, level, message))

print("ERROR Events:")
for ts, level, msg in events:
    if level == "ERROR":
        print(f"  {ts.strftime('%I:%M:%S %p')} — {msg}")

print(f"\nLog span: {events[-1][0] - events[0][0]}")
errors = [e for e in events if e[1] == "ERROR"]
if len(errors) >= 2:
    print(f"Time between errors: {errors[1][0] - errors[0][0]}")
print()

# ---------------------------------------------------------------------------
# 6B — Meeting Scheduler Across Timezones
# ---------------------------------------------------------------------------
print("--- 6B: Cross-Timezone Meeting Scheduler ---\n")

def find_overlap(tz_names, biz_start=9, biz_end=17):
    overlap = []
    for utc_hour in range(24):
        utc_dt = pytz.utc.localize(datetime(2026, 4, 29, utc_hour))
        local_times = {}
        all_ok = True
        for tz_name in tz_names:
            local_dt = utc_dt.astimezone(pytz.timezone(tz_name))
            local_times[tz_name] = local_dt
            if not (biz_start <= local_dt.hour < biz_end):
                all_ok = False
        if all_ok:
            overlap.append((utc_hour, local_times))
    return overlap

overlap = find_overlap(["US/Eastern", "Europe/London", "Asia/Tokyo"])
print("Overlap (9-5) for NY / London / Tokyo:")
if overlap:
    for utc_h, lt in overlap:
        parts = [f"{tz.split('/')[-1]}: {dt.strftime('%I %p')}" for tz, dt in lt.items()]
        print(f"  UTC {utc_h:02d}:00  →  {'  |  '.join(parts)}")
else:
    print("  None found!")
print()

# ---------------------------------------------------------------------------
# 6C — Event Countdown Dashboard
# ---------------------------------------------------------------------------
print("--- 6C: Event Countdown Dashboard ---\n")

upcoming = {
    "🎆 Independence Day": datetime(2026, 7, 4),
    "🎃 Halloween":        datetime(2026, 10, 31),
    "🦃 Thanksgiving":     datetime(2026, 11, 26),
    "🎄 Christmas":        datetime(2026, 12, 25),
    "🎊 New Year 2027":    datetime(2027, 1, 1),
}

now = datetime.now()
print(f"📅 Today: {now.strftime('%A, %B %d, %Y')}\n")
print(f"{'Event':<28} {'Days':>8}  {'Date':>16}")
print("-" * 56)
for name, dt in upcoming.items():
    d = (dt - now).days
    status = f"{d:>6} d" if d >= 0 else "PASSED"
    print(f"{name:<28} {status:>8}  {dt.strftime('%a, %b %d %Y'):>16}")
print()

# ---------------------------------------------------------------------------
# 6D — Date Range Generator
# ---------------------------------------------------------------------------
print("--- 6D: Date Range Generator ---\n")

def date_range(start, end, step=timedelta(days=1)):
    current = start
    while current <= end:
        yield current
        current += step

print("Mondays in May 2026:")
for d in date_range(datetime(2026, 5, 1), datetime(2026, 5, 31)):
    if d.weekday() == 0:
        print(f"  {d.strftime('%B %d (%A)')}")
print()
print("Bi-weekly paydays (Jun–Aug 2026):")
for d in date_range(datetime(2026, 6, 1), datetime(2026, 8, 31), timedelta(weeks=2)):
    print(f"  {d.strftime('%B %d, %Y (%A)')}")
print()


# ===========================================================================
# FINAL CHALLENGES (with Solutions)
# ===========================================================================
print("=" * 70)
print("  FINAL CHALLENGES")
print("=" * 70, "\n")

# ---------------------------------------------------------------------------
# CHALLENGE 1 — Flight Itinerary
# ---------------------------------------------------------------------------
print("--- Challenge 1: Flight Itinerary ---\n")

# ✅ SOLUTION
tz_et = pytz.timezone("US/Eastern")
tz_ct = pytz.timezone("US/Central")
tz_mt = pytz.timezone("US/Mountain")

f1_dep = tz_et.localize(datetime(2026, 4, 30, 6, 0))
f1_arr = tz_ct.localize(datetime(2026, 4, 30, 7, 15))
f2_dep = tz_ct.localize(datetime(2026, 4, 30, 9, 45))
f2_arr = tz_mt.localize(datetime(2026, 4, 30, 11, 0))

# All to UTC for accurate arithmetic
f1_dep_u = f1_dep.astimezone(pytz.utc)
f1_arr_u = f1_arr.astimezone(pytz.utc)
f2_dep_u = f2_dep.astimezone(pytz.utc)
f2_arr_u = f2_arr.astimezone(pytz.utc)

print(f"Flight 1 duration : {f1_arr_u - f1_dep_u}")
print(f"Layover           : {f2_dep_u - f1_arr_u}")
print(f"Flight 2 duration : {f2_arr_u - f2_dep_u}")
print(f"Total travel time : {f2_arr_u - f1_dep_u}")
print(f"Arrive Denver at  : {f2_arr.strftime('%I:%M %p %Z')}")
print(f"  (Home time ET)  : {f2_arr.astimezone(tz_et).strftime('%I:%M %p %Z')}")
print()

# ---------------------------------------------------------------------------
# CHALLENGE 2 — Work Hours & Pay Calculator
# ---------------------------------------------------------------------------
print("--- Challenge 2: Work Hours & Pay Calculator ---\n")

# ✅ SOLUTION
def calculate_pay(clock_in_str, clock_out_str, rate=25.00):
    ci = datetime.strptime(clock_in_str, "%Y-%m-%d %H:%M")
    co = datetime.strptime(clock_out_str, "%Y-%m-%d %H:%M")
    hrs = round((co - ci).total_seconds() / 3600, 2)
    ot_hrs = max(0, hrs - 8)
    reg_hrs = min(hrs, 8)
    reg_pay = reg_hrs * rate
    ot_pay = ot_hrs * (rate * 1.5)
    total = reg_pay + ot_pay

    print(f"  Clock in  : {clock_in_str}")
    print(f"  Clock out : {clock_out_str}")
    print(f"  Hours     : {hrs:.2f}  (OT: {'YES' if ot_hrs > 0 else 'No'})")
    if ot_hrs > 0:
        print(f"  Regular   : {reg_hrs:.2f}h × ${rate:.2f} = ${reg_pay:.2f}")
        print(f"  Overtime  : {ot_hrs:.2f}h × ${rate*1.5:.2f} = ${ot_pay:.2f}")
    print(f"  Total pay : ${total:.2f}\n")

calculate_pay("2026-04-29 08:30", "2026-04-29 17:45")
calculate_pay("2026-04-29 09:00", "2026-04-29 14:00")

# ---------------------------------------------------------------------------
# CHALLENGE 3 — Timezone Meeting Planner
# ---------------------------------------------------------------------------
print("--- Challenge 3: Timezone Meeting Planner ---\n")

# ✅ SOLUTION
def meeting_planner(participants, biz_start=8, biz_end=21):
    slots = []
    for utc_hour in range(24):
        utc_dt = pytz.utc.localize(datetime(2026, 4, 29, utc_hour))
        all_ok = True
        local_hours = []
        info = {}
        for name, tz_name in participants.items():
            local_dt = utc_dt.astimezone(pytz.timezone(tz_name))
            info[name] = local_dt
            local_hours.append(local_dt.hour)
            if not (biz_start <= local_dt.hour < biz_end):
                all_ok = False
        if all_ok:
            score = sum(abs(h - 12) for h in local_hours) / len(local_hours)
            slots.append((utc_hour, info, score))
    slots.sort(key=lambda x: x[2])
    return slots

participants = {
    "Alice":  "US/Eastern",
    "Bob":    "US/Pacific",
    "Carlos": "Europe/Madrid",
    "Diya":   "Asia/Kolkata",
}

slots = meeting_planner(participants)
if slots:
    print(f"Found {len(slots)} slot(s), ranked by convenience:\n")
    for rank, (utc_h, info, score) in enumerate(slots, 1):
        print(f"  #{rank} (score {score:.1f}) — UTC {utc_h:02d}:00")
        for name, dt in info.items():
            print(f"      {name}: {dt.strftime('%I:%M %p %Z')}")
        print()
else:
    print("  No available slots!")
print()


# ===========================================================================
# 🎓 QUICK REFERENCE CARD
# ===========================================================================
print("=" * 70)
print("  🎓 QUICK REFERENCE CARD")
print("=" * 70)
print("""
  from datetime import datetime, date, time, timedelta
  import pytz

  # NOW
  datetime.now()                            # naive local
  datetime.now(pytz.utc)                    # aware UTC
  datetime.now(pytz.timezone("US/Eastern")) # aware Eastern

  # CREATE
  datetime(2026, 4, 29, 14, 0, 0)          # naive
  tz.localize(naive_dt)                     # aware (pytz)
  datetime(..., tzinfo=ZoneInfo("..."))     # aware (3.9+)

  # FORMAT  →  dt.strftime("%Y-%m-%d %H:%M:%S")
  # PARSE   →  datetime.strptime(string, format)
  # CONVERT →  aware_dt.astimezone(other_tz)
  # MATH    →  dt + timedelta(days=7)   |   dt2 - dt1 → timedelta
  # COMPARE →  dt1 < dt2   |   dt1 == dt2   |   dt1 > dt2
""")
print("Happy coding! 🐍")
