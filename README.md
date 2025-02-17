# EveryHourBot

A Twitter and Mastodon bot written in NodeJS that powers @PossumEveryHour, using Twitter 1.1 API and cryptographic functionality for quality randomness.

Thanks to [IzzyMG](https://github.com/izzymg) for the help fixing up parts of the messy code.

## Dependencies
NodeJS 16
Twitter Developer Account with evelated permissions and access to Twitter V1 API

## How to use  
These instructions are for running the bot on a Linux/Windows server system. Code not designed to run on serverless infrastructure like AWS Lambda, Azure Functions or running on Heroku.  


1. [Clone the repository](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository) into a location you want it to run from 
2. Enter the directory where `index.js` is located and run `npm install` to install the required dependencies
3. Populate the `media` folder with pictures of your choosing that follow the [Twitter API Media Best Practices](https://developer.twitter.com/en/docs/twitter-api/v1/media/upload-media/uploading-media/media-best-practices).
4. [Apply for a Twitter developer account and obtain the v1 API and Access tokens from the account you want to run the bot from](https://developer.twitter.com/en/docs/twitter-api/getting-started/getting-access-to-the-twitter-api)  
5. Create a new `.env` file and add the required API keys. Use `.env-example` as an example file.  
6. Use a NodeJS daemon process like PM2 to start the bot. As an example, cd to the directory and run `pm2 start -f index.js --name "PossumEveryHour"`

If you are confused on where to start or you have no experience with Linux or servers in general, I'd suggest using something like a [Raspberry Pi](https://www.youtube.com/watch?v=BpJCAafw2qE), or [using an old computer to install Ubuntu Linux onto](https://www.youtube.com/watch?v=D4WyNjt_hbQ) and run your own bot from that. 

### Rocky Linux/RHEL8 requirements
Rocky/RHEL8 ships with NodeJS 10 by default. Execute these commands to disable the NodeJS 10 DNF module, enable NodeJS 16 module and install NodeJS 16:  

```
sudo dnf module disable nodejs:10 -y
sudo dnf module enable nodejs:16 -y
sudo dnf install @nodejs:16
```

Then verify:

```
$ node -v
v16.13.1
```

## How does it work?  

1. If executed from `index.js`, it will utilize [Node Schedule](https://www.npmjs.com/package/node-schedule), to execute `maincore.js` every hour, simulating cron job scheduling  
3. Code loads all the files in the `media` folder into an array  
2. Code verifies how many media files are in the `media` folder, and if it detects 2 or less images, it will refuse to run to prevent the code to be used for spamming  
3. `createPost()` function calls `getRandomFile()` that looks up how many entries are in the `usednumbers.txt` file depending on the count of files in `media` folder and if the file needs to be cleaned up.
4. `genRandomNumber()` function gets called, generating a number between 1 to count of files in `media` and checks against `usednumbers.txt` to see if that specific number was used in a specific timeframe. If it was used, repeat until a number that wasn't used is found.
5. Write the used number into `usednumbers.txt`


Tested with Rocky Linux 8.5, npm 8.1.2 and nodejs v16.13.1
