# Table of contents
- [Step 1: Enabling the FSRS Scheduler](#step-1-enabling-the-fsrs-scheduler)
- [Step 2: Personalizing FSRS](#step-2-personalizing-fsrs)
- [Configuring Different Parameters for Different Decks](#configuring-different-parameters-for-different-decks)
- [FAQ](#faq)

To get started with FSRS, you'll need to follow a two-step process.

- First, you'll need to enable the FSRS scheduler in your Anki application.
- Next, you'll need to personalize FSRS to suit your learning patterns.

Let's now discuss both of these steps in detail.

## Step 1: Enabling the FSRS Scheduler

### 1.1 Enable Anki's V3 Scheduler

Go to Tools > Preferences > Review > Enable V3 Scheduler.

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/8f91fba8-9b8b-405c-8aa9-42123ba5faeb"></p>

### 1.2 Paste FSRS Scheduler Code

- Go to the following page and copy all of the code. https://github.com/open-spaced-repetition/fsrs4anki/blob/main/fsrs4anki_scheduler.js
- In Anki, open the deck options of any deck (it doesn’t matter which deck). Find the Advanced Settings column, and paste the code you copied into the Custom Scheduling field:
<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/5c292f91-8845-4f8c-ac42-55f9a0f2946e"></p>

- Ensure that the learning and re-learning steps are shorter than 1 day in any deck you want to use with FSRS. Other settings, such as “Graduating interval” and “Easy interval”, don’t matter. For more details about which Anki settings matter and which are obsolete, see the [FAQs](https://github.com/open-spaced-repetition/fsrs4anki/wiki/FAQ).
<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/ba36847d-28f5-4df3-b4b3-4ff425609c04"></p>

After you perform the above steps, the FSRS4Anki Scheduler should ideally be active. If you want to confirm this, you can change this part of the code:

```javascript
const display_memory_state = false;
```

to:

```javascript
const display_memory_state = true;
```

Then open any deck for review and you'll see the following message:

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/0a5d4561-6052-45f3-91a5-5f21dd6497b9"></p>

This shows that the FSRS scheduler is running normally. If you don’t see D, S and R, and only see “FSRS enabled”, it means that the card is in the “learning” or “relearning” stage, not in the “review” stage.

You can then change the code back and the message will no longer display.

## Step 2: Personalizing FSRS

Personalizing FSRS for your learning needs involves a two-step process.

- First, you'll need to train the FSRS parameters for your collection using the FSRS optimizer, tailoring the algorithm to your learning patterns.
- Next, you'll need to choose the desired retention rate and maximum interval.

Let's now discuss both of these steps in detail.

### Step 2.1 Training the FSRS Parameters

For most users, it is advisable to use one of the following two methods (Google Colab and Hugging Face) for training the parameters. Advanced users can explore other options mentioned [here](https://github.com/open-spaced-repetition/fsrs4anki/wiki/Advanced-methods-of-optimization).

Note that the FSRS optimizer requires a minimum of 2,000 reviews to produce accurate results. If you don't have enough data, you can skip this step and use the default parameters instead, which are already entered into the scheduler code.

<details>
  <summary>Method 1: Training using Google Colab</summary>

Open the [optimizer's notebook](https://colab.research.google.com/github/open-spaced-repetition/fsrs4anki/blob/main/fsrs4anki_optimizer.ipynb). You don't need to configure the coding environment yourself and you can use Google's machines for free (you'll need to have a Google account):

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/5f5af21b-583d-496c-9bad-0eef0b1fb7a6"></p>

After the Colab website opens, switch to the folder tab. Once the Optimizer connects to Google's machines, you can right-click to upload your deck file/collection file exported from Anki.

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/66f9e323-fca8-4553-bcb2-b2e511fcf559"></p>

When exporting these files, make sure to select "Include scheduling information" and "Support older Anki versions". You don't need to include media.

<details>
  <summary>A note on Privacy</summary>
  
The decks that you upload to the optimizer can't be accessed by the author of FSRS. This can be verified by anyone who understands code because the code of the optimizer is open-source.

Google may have access to the uploaded data. But, the risk is similar to uploading the data to your personal Google Drive folder.

If you are too worried about privacy, you still have two options.
- Advanced users can run the script locally using the options mentioned [here](https://github.com/open-spaced-repetition/fsrs4anki/wiki/Advanced-methods-of-optimization).
- Other users can export their collection with blanked-out fields. To do this, go through the following steps:
    - Take a backup by going to `File → Create Backup` just in case anything goes wrong.
    - Go to `Browse > Notes > Find and Replace`.
    - Type `(.|\n)*` in the "Find" field and keep the "Replace With" field empty.
    - Check (✓) the "Treat input as regular expression" option. Uncheck "Selected notes only" if you want to apply this to all notes.<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/eaaf818d-e0b1-486f-875a-4aa6b96e258a"></p>
    - Export your collection using the steps mentioned above.
    - Restore the contents of your notes by going to `Edit → Undo Find and Replace`.

</details>

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/65da272d-7a01-4c46-a1d9-093e548f1a2d"></p>

- After uploading the file, replace the `collection-2022-09-18@13-21-58.colpkg` with the name of your uploaded file.
- Replace `Asia/Shanghai` with your timezone. The notebook contains a link to the list of time zones.
- Also, replace the value of `next_day_starts_at`. To find this value, Go to `Tools > Preferences > Review > Next day starts at` in your Anki. 

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/f344064c-4ccf-4884-94d0-fc0a1d3c3c24"></p>

Then, run the optimizer by either pressing `Ctrl+F9` or going to `Runtime > Run all`.

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/77947790-6916-4a99-ba28-8da42fd5b350"></p>

Wait for the code to finish running. Then, go to section 2.2 (Result), where the optimized parameters will be available. Copy these parameters.

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/8df1d210-73c3-4194-9b3b-256279c4c2fd"></p>
</details>

<details>
  <summary>Method 2: Training using Hugging Face</summary>
  
Simply upload your exported decks to this website and it will optimise it for you.  
https://huggingface.co/spaces/open-spaced-repetition/fsrs4anki_app

<p align="center"><img width="625" alt="image" src="https://github.com/Luc-mcgrady/fsrs4anki/assets/63685643/a03217f0-6627-4854-971f-f2bc9d14da5c"></p>
</details>

After training the parameters by either of the methods above, replace the parameters in the FSRS code that you copied earlier.

<p align="center"><img width="625" alt="image" src="https://github.com/open-spaced-repetition/fsrs4anki/assets/32575846/70b3b45a-f014-4574-81eb-cad6d19f93d9"></p>

⚠️Note: When replacing these parameters, make sure that you don't accidentally erase the square brackets or the comma after the closing bracket. The code will break without them.

Even after you start using FSRS, you should re-train the parameters once in every two months. However, it depends on how old your collection is. Users with relatively newer collections might want to re-optimize monthly. Re-optimization will ensure that FSRS works well with your current patterns of learning.

### Step 2.2: Choosing the desired retention rate and maximum interval

Now, you need to choose your `requestRetention`, which denotes the retention rate (i.e. the fraction of the cards recalled successfully) that FSRS will try to achieve. 

As an aid in deciding this value, you can view your past retention rate in Anki stats. For example, if your retention rate in the past was 90%, you can set 0.90 as your `requestRetention`.

You can set a higher `requestRetention` but keep in mind that as you increase the `requestRetention` above 0.90, the review load (reviews/day) will increase very rapidly. For the same reason, it is not advisable to use a `requestRetention` greater than 0.97.

After deciding the value of `requestRetention`, put this into the scheduler code. At the same time, decide the value of `maximumInterval`, which is the maximum interval any card is allowed to attain. The value in the FSRS scheduler code overrides the value set in Anki's deck options.

<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/6989b282-7988-4d9e-9fbe-0b79985e9952"></p>

After performing the above steps, you are ready to start using FSRS. Just start reviewing and FSRS will do its work.

### Using the FSRS4Anki Helper add-on to reschedule existing cards
After setting up FSRS in your Anki, you can install the [FSRS4Anki Helper add-on](https://ankiweb.net/shared/info/759844606) and use it to reschedule your existing cards. This is a one-time measure to reschedule the cards that were previously scheduled according to Anki's built-in algorithm. The add-on also offers many other useful features. Read more about the add-on here: https://github.com/open-spaced-repetition/fsrs4anki-helper

<p align="center"><img width="625" alt="image" src="https://github.com/user1823/fsrs4anki/assets/32575846/92289976-8b35-44b3-b5cd-3e6f89759c8d"></p>


## Configuring Different Parameters for Different Decks

You can also generate different parameters for different decks and configure them separately in the code. In the default configuration, `deckParams` already contains three groups of parameters.

The group "global config for FSRS4Anki" is global parameters.

The group "MainDeck1" are the parameters applied to the deck "MainDeck1" and its sub-decks.

Similarly, the third group is the parameters applied to the deck "MainDeck2::SubDeck::SubSubDeck" and its sub-decks. You can replace these with the decks you want to configure. If you need more, feel free to copy and add them.

```javascript
const deckParams = [
  {
    // Default parameters of FSRS4Anki for global
    "deckName": "global config for FSRS4Anki",
    "w": [0.4, 0.6, 2.4, 5.8, 4.93, 0.94, 0.86, 0.01, 1.49, 0.14, 0.94, 2.18, 0.05, 0.34, 1.26, 0.29, 2.61],
    // The above parameters can be optimized via FSRS4Anki optimizer.
    // For details about the parameters, please see: https://github.com/open-spaced-repetition/fsrs4anki/wiki/The-Algorithm
    // User's custom parameters for global
    "requestRetention": 0.9, // recommended setting: 0.75 - 0.95
    "maximumInterval": 36500,
    // FSRS only modifies the long-term scheduling. So (re)learning steps in deck options work as usual.
    // I recommend setting steps shorter than 1 day.
  },
  {
    // Example 1: User's custom parameters for this deck and its sub-decks.
    "deckName": "MainDeck1",
    "w": [0.6, 0.9, 2.9, 6.8, 4.72, 1.02, 1, 0.04, 1.49, 0.17, 1.02, 2.15, 0.07, 0.35, 1.17, 0.32, 2.53],
    "requestRetention": 0.9,
    "maximumInterval": 36500,
  },
  {
    // Example 2: User's custom parameters for this deck and its sub-decks.
    // Don't omit any keys.
    "deckName": "MainDeck2::SubDeck::SubSubDeck",
    "w": [0.6, 0.9, 2.9, 6.8, 4.72, 1.02, 1, 0.04, 1.49, 0.17, 1.02, 2.15, 0.07, 0.35, 1.17, 0.32, 2.53],
    "requestRetention": 0.9,
    "maximumInterval": 36500,
  }
];
```

If there are some decks you don't want to use FSRS with, you can add their names to the `skip_decks` list.

```javascript
const skip_decks = ["MainDeck3", "MainDeck4::SubDeck"];
```

# FAQ

Here, I have collected some frequently asked questions: [FAQ](https://github.com/open-spaced-repetition/fsrs4anki/wiki/FAQ)
