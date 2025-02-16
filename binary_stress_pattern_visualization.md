**mechveck**: _The following is the result of reading the first three pages of Bruno Munari's_ Design as Art_ . He lays out a brief description and diagram of a "useless machine." I turned to a chatbot to try see if they could mock up a more dynamic representation of what was on the page -- and I mentioned that I was an English language teacher. So English stress-patterning quickly emerged as the modality to try and get this into my head. Hopefully, it can not just start to demonstrate a phenomenon that English speakers are largely unaware of, but also demonstrate that human hands are still very much needed with these new machines. So far, even the most advanced language/transformer models cannot consistently identify "accurate" stress patterning. Which makes sense -- this is something that does not come naturally to native English speakers -- even for people who have studied it in the abstract. I've aimed for accuracy, and stuck with common vocabulary, but even then, the dictionaries and linguists and teachers etc. often disagree on the simplest terms._

*Generated and iterated with Anthropic's Claude and OpenAI's o1*

# Stress-Pattern Visualizer

[*Claude 3.5 + mechveck censor:*] A React component that visualizes stress patterns in English words, demonstrating how stress position can change meaning (e.g., noun vs. verb forms).

## Development Status: Workshop Phase

This project is currently in the Workshop phase of development. In this phase, we're:
- Implementing basic binary stress visualization (stressed/unstressed)
- Testing with simple noun/verb pairs (e.g., "present", "record")
- Preparing for initial Lab Testing with 2-3 students
- Following a "differentials over identity" principle, emphasizing how stress patterns change rather than static states

Next planned phase: Lab Testing with small student group to validate the visualization approach.

```jsx
import React, { useState } from 'react';

const StressVisualizer = () => {
  const patterns = {
    present: {
      noun: {
        syllables: ['pre', 'sent'],
        stress: [1, 0], // PRE-sent (gift)
        label: 'Present (gift)'
      },
      verb: {
        syllables: ['pre', 'sent'],
        stress: [0, 1], // pre-SENT (to show)
        label: 'Present (to show/demonstrate)'
      }
    },
    record: {
      noun: {
        syllables: ['re', 'cord'],
        stress: [1, 0], // RE-cord (documentation)
        label: 'Record (documentation)'
      },
      verb: {
        syllables: ['re', 'cord'],
        stress: [0, 1], // re-CORD (to capture audio/video)
        label: 'Record (to capture)'
      }
    }
  };

  const [currentWord, setCurrentWord] = useState('present');
  const [currentType, setCurrentType] = useState('noun');
  const [lastToggled, setLastToggled] = useState(Date.now());

  const toggleType = () => {
    setCurrentType(prevType => (prevType === 'noun' ? 'verb' : 'noun'));
    setLastToggled(Date.now());
  };

  const switchWord = () => {
    setCurrentWord(prevWord => (prevWord === 'present' ? 'record' : 'present'));
    setCurrentType('noun');
    setLastToggled(Date.now());
  };

  const { syllables, stress, label } = patterns[currentWord][currentType];

  return (
    <div className="flex flex-col items-center p-8 space-y-6">
      <div className="text-2xl font-bold">{label}</div>
      
      <div className="flex space-x-4">
        {syllables.map((syllable, index) => {
          const isStressed = Boolean(stress[index]);
          
          return (
            <div key={index} className="flex flex-col items-center">
              <div
                className={`
                  flex items-center justify-center rounded-full
                  ${isStressed ? 'bg-blue-500' : 'bg-gray-300'}
                  ${isStressed ? 'animate-pulse-slow' : 'animate-pulse'}
                `}
                style={{
                  width: isStressed ? '100px' : '60px',
                  height: isStressed ? '100px' : '60px'
                }}
              >
                <span
                  className={`text-white text-lg ${isStressed ? 'font-bold' : ''}`}
                >
                  {syllable}
                </span>
              </div>
              <span className="mt-2 text-sm text-gray-600">
                {isStressed ? 'STRESSED' : 'unstressed'}
              </span>
            </div>
          );
        })}
      </div>

      <div className="flex flex-col space-y-4">
        <button 
          onClick={toggleType}
          className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
        >
          Toggle Noun/Verb
        </button>
        <button 
          onClick={switchWord}
          className="px-4 py-2 bg-green-500 text-white rounded hover:bg-green-600"
        >
          Switch Word
        </button>
        
        <div className="text-sm text-gray-600 text-center">
          Last updated: {new Date(lastToggled).toLocaleTimeString()}
        </div>
        
        <div className="text-xs text-gray-500 italic">
          Currently displaying: <strong>{currentWord}</strong> as a <strong>{currentType}</strong>
        </div>
      </div>

      <style>{`
        @keyframes pulse {
          0%, 100% { transform: scale(1); }
          50% { transform: scale(1.1); }
        }
        
        @keyframes pulse-slow {
          0%, 100% { transform: scale(1); }
          50% { transform: scale(1.1); }
        }
        
        .animate-pulse {
          animation: pulse 1s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        
        .animate-pulse-slow {
          animation: pulse-slow 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
      `}</style>
    </div>
  );
};

export default StressVisualizer;
```

[*ChatGPT o1's Written-Out Visualization:*]
Here’s a conceptual “mental movie” of what’s happening in that code:

**1. Two circles side by side (for the two syllables).**  
- The **stressed** syllable circle is larger (100x100) and colored blue.  
- The **unstressed** circle is smaller (60x60) and gray.

---

**2. They both “pulse” in size, but at different speeds:**

- The **stressed** circle (blue) completes one pulse cycle in **2 seconds**:  
  1. It starts at normal size (scale: 1)  
  2. Slowly expands (scale: 1.1)  
  3. Returns to normal size (scale: 1)  
  4. Repeats…

- The **unstressed** circle (gray) completes its pulse cycle in **1 second**—effectively twice as fast:  
  1. Starts at normal size (scale: 1)  
  2. Expands (scale: 1.1)  
  3. Returns to normal size (scale: 1)  
  4. Repeats (and thus fits *two* cycles in the same 2 seconds that the stressed circle takes for *one*).

---

If you imagine them side-by-side on a timeline, it might look like this for a 2-second window:

Time   0s   0.5s   1.0s   1.5s   2.0s
|—–|—–|—–|—–|—–|
Stressed (2s cycle)
Scale   1    1.05   1.1   1.05   1  (then repeats)

Unstressed (1s cycle)
Scale   1    1.1    1     1.1    1  (then repeats)

- The **stressed** circle is gently pulsing: from size 1 → up to 1.1 → back to 1 over 2 full seconds.  
- The **unstressed** circle is pulsing at double speed: it will do that expand–contract cycle twice in the same amount of time.

---

**3. “Noun” vs. “Verb” stress changes which circle is big/blue**  
- For **“present”**:  
  - Noun = **PRE**-sent → first syllable is stressed (the first circle is bigger, blue, slower pulse).  
  - Verb = pre-**SENT** → second syllable is stressed (the second circle is bigger, blue, slower pulse).  

- For **“record”**:  
  - Noun = **RE**-cord → first syllable is stressed.  
  - Verb = re-**CORD** → second syllable is stressed.

When you click “Toggle Noun/Verb” or “Switch Word,” it flips which circle is bigger/blue (stressed) vs. smaller/gray (unstressed), and the pulsing speeds stay the same logic: stressed circle = slow pulse, unstressed circle = fast pulse.

---

**In short:**  
- **Blue circle** = stressed syllable, bigger, **2-second** expand-and-contract animation.  
- **Gray circle** = unstressed syllable, smaller, **1-second** expand-and-contract animation.  
- Together, they look like they’re “breathing” at different tempos—a neat visual of stress timing in pronunciation!
