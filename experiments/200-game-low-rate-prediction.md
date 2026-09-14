# Prediction recorded before the lower-rate run

The 200-game run at learning rate 0.0001 fell to a five-game mean of 600, and its training loss rose. Changing only the learning rate to 0.00005 may produce steadier updates and improve the five-game mean; it could also learn too slowly. This run keeps 200 episodes, 0.20 training exploration, and the fixed five evaluation seeds, 5% evaluation exploration, and 3,000-decision limit.
