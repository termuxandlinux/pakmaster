#!/usr/bin/env python3
import sys
import os
import subprocess

DATABASES_DIR = os.path.expanduser("~/.pakmaster/db/")  # Directory to store databases


def load_database(database_name):
    database_path = os.path.join(DATABASES_DIR, f"{database_name}.db")
    if not os.path.exists(database_path):
        print(f"Database '{database_name}' not found.")
        return
    print(f"Switching to database '{database_name}'.")
    # Load or update the database from the file system
    with open(database_path, "r") as f:
        for line in f:
            package, url = line.strip().split(",")
            print(f"Package: {package}, URL: {url}")


def add_database(database_name, url):
    database_path = os.path.join(DATABASES_DIR, f"{database_name}.db")
    if os.path.exists(database_path):
        print(f"Database '{database_name}' already exists.")
        return
    print(f"Adding database '{database_name}' with URL '{url}'.")
    # Dummy implementation: In a real scenario, fetch the database from the provided URL
    # and store it in your system
    with open(database_path, "w") as f:
        f.write(f"{database_name},{url}\n")
    print(f"Database '{database_name}' added successfully.")


def install_package(package_name):
    # Search for the package in the current database
    for database_file in os.listdir(DATABASES_DIR):
        database_name = os.path.splitext(database_file)[0]
        database_path = os.path.join(DATABASES_DIR, database_file)
        with open(database_path, "r") as f:
            for line in f:
                name, url = line.strip().split(",")
                if name == package_name:
                    print(f"Downloading and installing package '{package_name}' from '{url}'.")
                    # Download the package
                    subprocess.run(["wget", url])
                    # Install the package
                    subprocess.run(["dpkg", "-i", f"{package_name}.deb"])
                    return
    print(f"Package '{package_name}' not found in the current database.")


def search_package(package_name):
    # Search for the package in the current database
    for database_file in os.listdir(DATABASES_DIR):
        database_name = os.path.splitext(database_file)[0]
        database_path = os.path.join(DATABASES_DIR, database_file)
        with open(database_path, "r") as f:
            for line in f:
                name, url = line.strip().split(",")
                if name == package_name:
                    print(f"Package '{package_name}' found in the current database.")
                    return
    print(f"Package '{package_name}' not found in the current database.")


def display_help():
    print("Usage: pakmaster [OPTIONS] COMMAND [ARGS]...")

    print("\nOptions:")
    print("--help      Show this message and exit.")
    print("--cache     Create the cache directory '~/.pakmaster/db/'.")

    print("\nCommands:")
    print("install     Install a package.")
    print("search      Search for a package.")
    print("add         Add a database.")
    print("change-db   Change current database.")


def main():
    if "--cache" in sys.argv:
        os.makedirs(DATABASES_DIR, exist_ok=True)
        print(f"Cache directory created: {DATABASES_DIR}")
        sys.exit(0)

    if len(sys.argv) < 2 or sys.argv[1] == "--help":
        display_help()
        sys.exit(0)

    command = sys.argv[1]
    if command == "install":
        if len(sys.argv) < 3:
            print("Please provide a package name.")
        else:
            install_package(sys.argv[2])
    elif command == "search":
        if len(sys.argv) < 3:
            print("Please provide a package name.")
        else:
            search_package(sys.argv[2])
    elif command == "add":
        if len(sys.argv) < 4:
            print("Please provide a database name and URL.")
        else:
            add_database(sys.argv[2], sys.argv[3])
    elif command == "change-db":
        if len(sys.argv) < 3:
            print("Please provide a database name.")
        else:
            load_database(sys.argv[2])
    else:
        print("Invalid command. Use 'pakmaster --help' for usage instructions.")


if __name__ == "__main__":
    main()
