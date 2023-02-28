# CS370-Emerging-Trends



CS370 Emerging Trends- Project 2 
README

•	Briefly explain the work that you did on this project: What code were you given? What code did you create yourself?

The TreasureHuntGame is a pathfinding AI project. It is a game where the player needs to find the treasure before the AI pirate agent does. Using a deep Q-learning algorithm, the pirate learns the optimal way to find the treasure from any position on the map. I was provided with two Python classes and a notebook to complete the task. The first class, TreasureMaze.py, represents the environment, which includes a maze object defined as a matrix. The second class, GameExperience.py, stores the episodes  which means all the states that come in between the initial state and the terminal state. This is later used by the agent for learning by experience, called "exploration". 
I was to design code to reflect pseudocode provided in the form of a #TODO task that would determine the optimal number of epochs to achieve a 100% win rate.
The pseudocode given for the TreasureHuntGame:

# pseudocode:
    # For each epoch:
    #    Agent_cell = randomly select a free cell
    #    Reset the maze with agent set to above position
    #    Hint: Review the reset method in the TreasureMaze.py class.
    #    envstate = Environment.current_state
    #    Hint: Review the observe method in the TreasureMaze.py class.
    #    While state is not game over:
    #        previous_envstate = envstate
    #        Action = randomly choose action (left, right, up, down) either by exploration or by exploitation
    #        envstate, reward, game_status = qmaze.act(action)
    #    Hint: Review the act method in the TreasureMaze.py class.
    #        episode = [previous_envstate, action, reward, envstate, game_status]
    #        Store episode in Experience replay object
    #    Hint: Review the remember method in the GameExperience.py class.
    #        Train neural network model and evaluate loss
    #    Hint: Call GameExperience.get_data to retrieve training data (input and target) and pass to model.fit method 
    #          to train the model. You can call model.evaluate to determine loss.
    #    If the win rate is above the threshold and your model passes the completion check, that would be your epoch.


The code I completed:

# For loop used for games. Each loop/epoch is a game
    for epoch in range(n_epoch):
        print('\n\n\n')
        print('This is the start of the next epoch...')
        print('\n')
        # Randomly select free cell to start agent in. this is an array with length of 2
        agent_cell = np.random.randint(0, high=7, size=2)
        # Reset the game with agent at the random starting position
        qmaze.reset([0,0])
        envstate = qmaze.observe()
        loss = 0
        n_episodes = 0
        
        # while game is not over
        while qmaze.game_status() == 'not_over':
            previous_envstate = envstate
            valid_actions = qmaze.valid_actions()
            
            # Get next action
            if np.random.rand() < epsilon:
                action = random.choice(valid_actions)
            else:
                action = np.argmax(experience.predict(envstate))
            
            # Take the action
            envstate, reward, game_status = qmaze.act(action)
            # Increase episode count because taking an action makes an episode
            n_episodes += 1
            # Remember the episode
            episode = [previous_envstate, action, reward, envstate, game_status]
            experience.remember(episode)                
                
            inputs,targets = experience.get_data()            
            history = model.fit(inputs, targets, epochs=8, batch_size=24, verbose=0)
            loss = model.evaluate(inputs, targets)
            
            # if pirate agent won game
            if episode [4] == 'win':
                win_history.append(1)
                win_rate = sum(win_history) / len(win_history)
                break
            # if pirate agent lost the game
            elif episode[4] == 'lose':
                win_history.append(0)
                win_rate = sum(win_history) / len(win_history)
                break
            # else the game is not over
        
        if win_rate > epsilon:
            print('win rate is larger than epsilon!!!!')
            if completion_check(model, qmaze) == True:
                print('completion_check() passes!!')


Connect your learning from throughout this course to the larger field of computer science:

	What do computer scientists do and why does it matter?
  
Computer scientists design and develop software, algorithms, and applications. They also conduct research on topics such as artificial intelligence, big data, and cybersecurity. Computer scientists develop algorithms and theories that help create efficient and effective software solutions. They also work on improving hardware design and performance. In addition, computer scientists conduct research on a variety of topics, such as artificial intelligence, human-computer interaction, and information management. Computer science is a relatively new field, and it is constantly evolving. As our world becomes more reliant on technology, the need for computer scientists will only continue to grow.
•	
	How do I approach a problem as a computer scientist?
  
As a computer scientist, the approach to problem-solving should involve breaking down complex problems into smaller, more manageable components. This technique is called decomposition. A Computer Scientist will then identify the required inputs and desired outputs for each component and develop an algorithm or code to carry out the necessary computation. Testing and debugging are also important aspects of problem-solving in computer science. Effective problem-solving in computer science also requires knowledge of programming languages, algorithms, data structures, and computer architecture.

	What are my ethical responsibilities to the end user and the organization?

•	Computer scientists have ethical responsibilities towards the end-user and the organization they work for. These responsibilities include designing software and systems that are secure, reliable, and user-friendly. Computer scientists should also be aware of the impact of their work on society and consider the potential risks and benefits of their technology. Privacy and data protection are also critical ethical concerns in computer science.


