# Name of the file to be downloaded
FILE = myfile.txt

# Expected MD5 checksum of the file
EXPECTED_MD5SUM = 68b329da9893e34099c7d8ad5cb9c940

# Path to t4mf-md5sum-check-ok script
SCRIPT_PATH = ../../bin/t4mf-md5sum-check-ok

.PHONY: download check-md5sum clean

# This is just and example. Downloading files from the Internet for a build
# is not appropriate for all software. The md5 check script can of course also
# check the result of a build process with a md5 checksum. However for this
# example we consider a download.
# Rule to download the file (this is just a placeholder, replace it with the
# actual command to download the file). 
download: 
	@echo "Downloading $(FILE)..."
	@echo "" > $(FILE) 

# Rule to check the MD5 checksum of the downloaded file
check-md5sum: download
	@echo "Checking MD5 checksum of $(FILE)..."
	@echo "Expected MD5 checksum is $(EXPECTED_MD5SUM)"
	@RESULT=$$($(SCRIPT_PATH) $(FILE) $(EXPECTED_MD5SUM)); \
	if [ $$? -eq 0 ]; then \
		echo "MD5 checksum OK, continuing..."; \
	elif [ $$? -eq 1 ]; then \
		echo "MD5 checksum mismatch, stopping the build..."; \
		exit 1; \
	elif [ $$? -eq 2 ]; then \
		echo "Incorrect usage of script, stopping the build..."; \
		exit 1; \
	elif [ $$? -eq 3 ]; then \
		echo "File does not exist, stopping the build..."; \
		exit 1; \
	else \
		echo "Unknown error, stopping the build..."; \
		exit 1; \
	fi

clean:
	@echo "Cleaning up..."
	rm -f $(FILE)

