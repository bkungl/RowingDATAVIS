---
id: litvis

narrative-schemas:
  - ../narrative-schemas/proposal.yml
---

@import "../css/datavis.less"

# Coursework: Project Proposal
------------------
# Feedback
I like the theme you have chosen. Any one of you RQs could potentially be answered without visualization, but their combination gives you a nice basis for visual exploration and you justification paragraph hints at this.
------------------

{(questions|}

A couple different ideas:

- What is the likelyhood of an elite rower winning a race after winning the 1st, 2nd, and 3rd 500m marks in a 2km race?
- What is the optimal race structure to win the World Rowing Championships in the Lightweight Men's Single; i.e. is it best to go off hard and try to win from the start, or sit at the back of the pack and puta big push in near the end of the race to take the lead?
- What is the ideal height of an elite lightweight rower between different boat classes given that they all weigh in at the same weight?

{|questions)}

{(datasources|}

_A brief sentence or two with links to data sources._

{|datasources)}

I can get almost all the information I need from worldrowing.com. They provide not only competitior information including height, but also speed and stroke rate race data for every elite race. Every 50 meters of a 2000m race data is provided and looks something like: http://www.worldrowing.com/assets/pdfs/WCH_2019_1/ROWMSCULL1-L----------FNL-000100--_C77.pdf

This data can be converted to more readable format than PDF by using a PDF to excel document and they cutomizing a parsing script to change the data into an appropriate csv to make use of.

{(justification|}

_A couple of sentences on why you need datavis to answer your research questions._

{|justification)}

There are many factors to trying to answer the above questions beyond just looking at the world best time and saying that the athlete had the perfect build and race plan to succeed. There are many elements that determine the result of a rowing race, including, but not limited to: water termperature, air temperature, wind direction, wind speed, water flow rate, air quality, boat size, etc.

I am excited about trying to find research ideas related to high level rowing because it interests me and these are things that every rowing coach thigns something different about. It would be interesting to try to find imperical truths about rowing that have gone unanswered.
