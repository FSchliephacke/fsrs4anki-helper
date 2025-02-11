# FSRS4Anki Helper

[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

FSRS4Anki Helper is an Anki add-on that supports [FSRS4Anki](https://github.com/open-spaced-repetition/fsrs4anki) scheduler. It has six main features:

- **Reschedule** cards based on their entire review histories.
- **Postpone** a selected number of due cards.
- **Advance** a selected number of undue cards.
- **Balance** the load during rescheduling (based on fuzz).
- **Less Anki** on Easy Days (such as weekends) during rescheduling (based on load balance).
- **Disperse** Siblings (cards with the same note) to avoid interference & reminder.

# Requirements

- For Anki version in 2.1.55 - 2.1.66 
  - Enable V3 Scheduler
  - FSRS4Anki scheduler version >= 3.0.0
- For Anki version >= 23.10
  - Enable built-in FSRS
  - Remove FSRS4Anki scheduler code if you used it before

# Installation

The FSRS4Anki Helper add-on is purely an added bonus and is not recommended for extensive use.

Installation link: https://ankiweb.net/shared/info/759844606

# Usage

## Overview

| Feature name      | How does it work?                                            | When should I use it?                                        |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Reschedule        | Calculates stability, difficulty, and the optimum interval from the entire review history for each card using FSRS parameters. Then, it changes the due dates of cards. | When you update the weights or other parameters of FSRS. However, this is not necessary anymore, as Anki has a built-in feature "Reschedule cards on change". |
| Advance           | Decreases the intervals of undue cards based on current and requested R, and interval length to minimize damage to long-term learning. | When you want to review your material ahead of time, for example, before a test. |
| Postpone          | Increases the intervals of cards that are due today based on current and requested R, and interval length in a way that minimizes damage to long-term learning. | When you are dealing with a large number of reviews after taking a break from Anki or after rescheduling. |
| Load Balancing    | After the optimal interval is calculated, it is adjusted by a random amount to make the distribution of reviews over time more uniform. | Always. This feature makes your workload (reviews per day) more consistent. |
| Easy Days         | After the optimal interval is calculated, it is slightly adjusted to change the due date. | If you want to spend less time on Anki on some days of the week, for example, Sundays. |
| Disperse Siblings | Siblings are cards generated from the same note. Their intervals are adjusted to spread them further apart from each other. | Always. This feature alleviates the interference; disabling it will only decrease the efficiency of spaced repetition. |

## Reschedule

Rescheduling can calculate the memory states and intervals based on each card's review history and the parameters from the Scheduler code. These parameters can be personalized with the FSRS Optimizer.

**Note**: For cards that have been reviewed multiple times using Anki's default algorithm, rescheduling may give different intervals than the Scheduler because the Scheduler can't access the full review history when running. In this case, the intervals given by rescheduling will be more accurate. But after rescheduling once, there will be no difference between the two.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/7ec8710f-66ac-47bf-b498-13917944ec9a)

## Advance/Postpone

These two functions are very similar, so I'll talk about them together. You can set the number of cards to advance/postpone, and the Helper add-on will sort your cards and perform the advance/postpone in such a way that the deviation from the original review schedule is minimal while meeting the number of cards you set.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/303912eb-2645-4c75-a554-2c76024744f2)

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/f9838010-cb00-44ce-aefc-10300f2a586e)

## Load Balance

Once the load balance option is enabled, rescheduling will make the daily review load as consistent and smooth as possible.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/4ac4f5aa-e4c6-4f50-b30c-1595f930d2f3)

Here's a comparison, the first graph is rescheduling before enabling it, and the second graph is after enabling:

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/1f31491c-7ee6-4eed-ab4a-7bc0dba5dff8)

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/1c4f430d-824b-4145-801e-68fc0329fbbd)

## Easy Days

You can choose any day or days from Monday to Sunday to take off. Once enabled, the Helper will try to avoid these days when rescheduling. Note: Easy Days only works for review cards. Due to technical limitations, FSRS doesn't modify the interval and due date of (re)learning cards. And it also doesn't reschedule cards whose interval is less than 3 days to respect the desired retention.

Fuzz example:
- Review less than 3 days: will not choose another day.
- Review in 3 days: Choose between days 2 and 4.
- Review in 7 days: Choose between days 5 and 9.
- Review in 90 days: Choose between days 86 and 94.
- Cards you forgot, and return within 2 days it does not choose another day.

Exceptions:

If the fuzz range is too narrow or does not exist (review less than 3 days) to satisfy the selected easy days, the day of the week may be selected for review of the card.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/4df36a3c-6c5e-4f1f-81ac-1740c7fe15bb)

**Effect**:

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/7fe6b4d0-ae99-40f8-8bd9-0f7c3ff1c638)

## Disperse Siblings

In Anki, some templates will generate multiple cards related in content from the same note, such as reversed cards (Front->Back, Back->Front) and cloze cards (when you make multiple clozes on the same note). If the review dates of these cards are too close, they may interfere with or remind you of each other. Dispersing siblings can spread the review dates of these cards out as much as possible.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/b33460f4-d7db-4c8f-b9d0-d0193f2d1f54)

## Advanced Search (<=2.1.66)

In the card browser, you can right-click on the header and click on Difficulty, Stability, Retention to display the current memory states of cards.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/7fb2b357-19d0-45fb-9cc0-f925258a6280)

The Helper also adds [search syntax](https://docs.ankiweb.net/searching.html) for these three attributes, here are some examples:

- s<10: Cards with memory stability less than 10 days
- d=5: Cards with difficulty equal to 5
- r<0.6: Cards with memory retrievability (recall probability) less than 60%

## Advanced Statistics

Hold down the Shift key and click "Stats" to enter the old version of Anki's statistics interface.

![image](https://github.com/open-spaced-repetition/fsrs4anki-helper/assets/32575846/db368bcb-54a5-4ca2-bc14-acad382f643f)

The FSRS Stats are based on all cards in your deck or collection (whichever is selected) that you have ever reviewed. They remain unaffected by the 1 month/year settings.

Interpretation:

- **Daily Load** is an estimate of the average review count required per day. It is influenced by the [Burden](https://supermemo.guru/wiki/Burden) statistic in SuperMemo. 
- The [three component of the memory model](https://supermemo.guru/wiki/Three_component_model_of_memory) used by FSRS:
	- Average predicted **retention** reflects the percentage of cards that you would recall correctly if you were tested today.
	- Average (memory) **stability** reflects how fast you forget (forgetting rate). The greater the stability, the slower the forgetting rate.
	- **Difficulty** reflects how hard it is to increase or maintain the stability of a memory. Its relative distribution within the deck/collection can be viewed at the bottom of the statistics interface (<=2.1.66):
	![image](https://user-images.githubusercontent.com/32575846/260213063-9b18fbaa-6b92-4392-8984-03b85f3fcedd.png)

## Other features
- **Auto reschedule cards reviewed on other devices after sync:** This option is useful if you do some (or all) of your reviews on platforms that don't support FSRS such as AnkiDroid or AnkiWeb. If this option is enabled, the reviews synced from the other devices will be automatically rescheduled according to the FSRS algorithm. If you are relying on this feature, it is recommended to sync the reviews daily for the best results.
- **Auto reschedule the card you just reviewed (<=2.1.66):** If you enable this option, every card that you review will be rescheduled. Enabling this option is not essential for using FSRS. It is mainly intended for gradually transitioning your old cards to FSRS when starting to use FSRS. The other option for transitioning old cards to FSRS is to reschedule all cards, but this tends to induce a huge backlog for many people. Other advantages of enabling the "Auto reschedule the card you just reviewed" option include:
    - Load balance and easy days are applied when rescheduling.
    - It allows you to use learning or relearning steps longer than or equal to 1 day without breaking the scheduling. However, for best results, it is not recommended to use such steps even with this option enabled because FSRS can determine the next intervals more accurately.

    However, this option also has some disadvantages, which include:
    - The intervals displayed above the answer buttons may be inconsistent with the real interval after rescheduling, though the real ones will be more optimal.
    - It might have a small effect on the responsiveness of Anki and introduce lags because it needs more calculations for each review and causes constant queue rebuilding.
    - If it is enabled, after answering a card, it requires two undo key presses to actually undo answering the card.
- **Auto disperse siblings when review:** It automatically disperses siblings after each review. But it could cause constant queue rebuilding, which slows down Anki and breaks Display Order settings.
- **Reschedule all cards:** This option is used to reschedule all the cards in the decks in which FSRS is enabled. It should only be used after you have installed FSRS for the first time and/or updated your parameters.
- **Reschedule cards reviewed in the last 7 days:** This option can be used to reschedule the cards that were reviewed in the last few days. The number of days can be adjusted in the add-on config.
- **Update scheduler:** This option can be used to check for updates to the FSRS scheduler and update the scheduler code if an update is available. While updating the scheduler code, this option preserves your existing configuration.

# Mechanism

Please see this wiki page: [FSRS4Anki Helper WIKI](https://github.com/open-spaced-repetition/fsrs4anki-helper/wiki)
