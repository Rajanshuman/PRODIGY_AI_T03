# PRODIGY_AI_T03
# Implement a simple text generation algorithm using Markov chains. This task involves creating a statistical model that predicts the probability of a character or word based on the previous one(s).

A Markov chain is a mathematical system usually defined as a collection of random variables, that transition from one state to another according to certain probabilistic rules. These set of transition satisfies the Markov Property, which states that the probability of transitioning to any particular state is dependent solely on the current state and time elapsed, and not on the sequence of state that preceded it. This unique characteristic of Markov processes render them memoryless.
A Markov chain is a random process with the Markov property. A random process or often called stochastic property is a mathematical object defined as a collection of random variables. A Markov chain has either discrete state space (set of possible values of the random variables) or discrete index set (often representing time) - given the fact, many variations for a Markov chain exists. Usually the term "Markov chain" is reserved for a process with a discrete set of times, that is a Discrete Time Markov chain (DTMC).

Let's try to code the example above in Python. And although in real life, you would probably use a library that encodes Markov Chains in a much efficient manner, the code should help you get started...
first we install some preferable library for implementing morkov chain. 

import random

class MarkovChain:
    def __init__(self):
        self.memory = {}

    def _learn_key(self, key, value):
        if key not in self.memory:
            self.memory[key] = {}

        self.memory[key][value] = self.memory[key].get(value, 0) + 1

    def learn(self, text):
        for i in range(1, len(text)):
            self._learn_key(text[i-1], text[i])

    def _next(self, current_state):
        possible_next = self.memory[current_state]
        total_count = sum(possible_next.values())
        probs = {k: v/total_count for k, v in possible_next.items()}
        return random.choices(list(probs.keys()), list(probs.values()))[0]

    def generate_text(self, start_char, length):
        result = start_char
        for _ in range(length - 1):
            result += self._next(result[-1])
        return result

# Example usage
mc = MarkovChain()
text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum."
mc.learn(text)

print(mc.generate_text('D', 100))

You get a random set of transitions possible along with the probability of it happening, starting from state: Sleep. Extend the program further to maybe iterate it for a couple of hundred times with the same starting state, you can then see the expected probability of ending at any particular state along with its probability.

