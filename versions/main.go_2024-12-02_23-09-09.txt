package main

import (
	"fmt"
	"io/ioutil"
	"os"
	"time"
)

func main() {
	// Welcome message
	fmt.Println("Welcome to Simple VCS!")

	// Get the file name and the operation from the user
	var filename string
	fmt.Print("Enter the file name to track: ")
	fmt.Scan(&filename)

	// Check if the file exists
	if _, err := os.Stat(filename); os.IsNotExist(err) {
		fmt.Println("File does not exist. Please create the file first.")
		return
	}

	// Read the file content
	content, err := ioutil.ReadFile(filename)
	if err != nil {
		fmt.Println("Error reading file:", err)
		return
	}

	// Create a version directory if it doesn't exist
	versionsDir := "versions"
	if _, err := os.Stat(versionsDir); os.IsNotExist(err) {
		err = os.Mkdir(versionsDir, os.ModePerm)
		if err != nil {
			fmt.Println("Error creating versions directory:", err)
			return
		}
	}

	// Save the file version with a timestamp
	timestamp := time.Now().Format("2006-01-02_15-04-05")
	versionFilename := fmt.Sprintf("%s/%s_%s.txt", versionsDir, filename, timestamp)

	err = ioutil.WriteFile(versionFilename, content, 0644)
	if err != nil {
		fmt.Println("Error saving version:", err)
		return
	}

	// Success message
	fmt.Printf("Version saved as: %s\n", versionFilename)
}
