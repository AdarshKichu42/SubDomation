#!/bin/bash

# Usage: ./subenum.sh target.com

domain=$1

if [ -z "$domain" ]
then
    echo "Usage: $0 domain.com"
    exit 1
fi

echo "[+] Starting Subdomain Enumeration for $domain"

mkdir -p recon/$domain
cd recon/$domain

echo "[+] Running Subfinder..."
subfinder -d $domain -silent > subfinder.txt

echo "[+] Running Assetfinder..."
assetfinder --subs-only $domain > assetfinder.txt

echo "[+] Running Amass (passive)..."
amass enum -passive -d $domain > amass.txt

echo "[+] Combining results..."
cat subfinder.txt assetfinder.txt amass.txt | sort -u > all_subdomains.txt

echo "[+] Checking live domains..."
cat all_subdomains.txt | httprobe > live_subdomains.txt

echo "[+] Enumeration completed!"

echo "All Subdomains: recon/$domain/all_subdomains.txt"
echo "Live Subdomains: recon/$domain/live_subdomains.txt"# SubDomation
