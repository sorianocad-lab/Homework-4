/* adventure_toolkit.py */

"""Adventure Toolkit — Chapters 1–3,5
Authors: <Diego.S>, 
Course/Section: CS/CMPE 115, <Term>
Description: Prep utilities (time/coins/eligibility) + mini-adventure with recursion and conditionals.
"""

def seconds_to_hms(total_seconds):
    """Return (hours, minutes, seconds) for a non-negative total_seconds."""
    # TODO: validate non-negative
    h = total_seconds // 3600
    rem = total_seconds % 3600
    m = rem // 60
    s = rem % 60
    return h, m, s

def copper_to_coins(cp):
    """Return (gp, sp, cp) using 1 gp = 100 sp and 1 sp = 10 cp."""
    sp_total = cp // 10
    cp_left  = cp % 10
    gp = sp_total // 100
    sp = sp_total % 100
    return gp, sp, cp_left

def ask_int(prompt):
    """Recursively prompt until user enters an int; return the int."""
    s = input(prompt)
    if s.strip() == '':
        print("Please enter a whole number.")
        return ask_int(prompt)  # recurse (progress: user re-enters)
    try:
        return int(s)
    except ValueError:
        print("Please enter a whole number.")
        return ask_int(prompt)

def ask_choice(prompt, options):
    """Recursively prompt until input is one of options (strings)."""
    s = input(prompt).strip().lower()
    if s in options:
        return s
    print(f"Please enter one of: {', '.join(options)}")
    return ask_choice(prompt, options)  # recurse

def guess_guard_code(secret, attempts_left):
    """Return True if user guesses secret within attempts_left; else False (recursive)."""
    if attempts_left == 0:
        return False  # base case
    guess = ask_int(f"Enter the 2-digit guard code ({attempts_left} attempts): ")
    if guess == secret:
        return True
    elif guess < secret:
        print("Too low.")
    else:
        print("Too high.")
    return guess_guard_code(secret, attempts_left - 1)  # progress

def intro_scene(name):
    print(f"\nWelcome, {name}. The torches flicker as you enter the ruins…")

def fork_scene(has_torch):
    print("\nYou reach a fork: left path is windy; right path is silent.")
    path = ask_choice("Choose (left/right): ", {"left", "right"})
    # Example of nested conditionals
    if path == "left":
        if has_torch:
            print("Your torch reveals markings that guide you safely.")
            return "left_safe"
        else:
            print("Darkness hides a pit. You stumble but recover.")
            return "left_risky"
    else:
        if has_torch:
            print("You spot a hidden alcove with an iron gate.")
            return "right_gate"
        else:
            print("You hear water ahead. The ground is slick.")
            return "right_slick"

def ending_scene(tag):
    # Chained / nested conditionals demo
    if tag == "right_gate":
        ok = guess_guard_code(secret=77, attempts_left=3)
        if ok:
            print("Success! The gate opens. Treasure glitters beyond.")
        else:
            print("Alas, the mechanism locks with a clang.")
    elif tag == "left_safe":
        print("You follow the markings to a sunlit exit. Freedom!")
    elif tag == "left_risky":
        print("Bruised but wiser, you decide to return another day.")
    else:  # right_slick
        print("A shallow stream bars your path; you turn back.")

def run_prep_utilities():
    print("=== Prep Utilities ===")
    secs = ask_int("Enter total seconds: ")
    h, m, s = seconds_to_hms(secs)
    print(f"H:MM:SS = {h}:{m:02d}:{s:02d}")

    cp = ask_int("Enter copper pieces (cp): ")
    gp, sp, cp2 = copper_to_coins(cp)
    print(f"Coins → gp:{gp}  sp:{sp}  cp:{cp2}")

    age = ask_int("Enter age: ")
    citizen = ask_choice("Citizen (y/n): ", {"y", "n"})
    is_eligible = (age >= 18) and (citizen == "y")
    print("Eligible?", is_eligible)

def run_adventure():
    print("\n=== Mini-Adventure ===")
    name = input("What is your name, adventurer? ")
    torch_choice = ask_choice("Do you carry a torch? (y/n): ", {"y", "n"})
    has_torch = (torch_choice == "y")

    # DEBUG: show initial state (comment out if you prefer)
    print(f"# DEBUG: name={name}, has_torch={has_torch}")

    intro_scene(name)
    tag = fork_scene(has_torch)
    ending_scene(tag)

def main():
    print("Adventure Toolkit — Chapters 1–3,5")
    run_prep_utilities()
    run_adventure()

if __name__ == "__main__":
    main()
