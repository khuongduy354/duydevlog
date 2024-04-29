---
title: "Pikachu Game - Simple yet Algorithmic"
date: 2024-04-30T00:53:02+07:00
draft: false
---


# Pikachu matching game - Simple yet algorithmic 
> A simple Puzzle game that packs quite a bunch in Algorithm & Data structure in Programming. 

![](https://st.gamevui.com/images/image/2017/05/06/pikachu-hd2.jpg)
### Game rules 
Pick 2 pokemons with the exact same images, if the path between the 2 isn't blocked and obey following rule: 
>path formed shoudn't have more than 3 edges (in other word: have more than 2 turning points)

Below are the valid path shapes, they have I, L, U, Z shapes... 

![](https://scontent.xx.fbcdn.net/v/t1.15752-9/438204593_3789105254641855_4752512429503022204_n.png?stp=dst-png_p296x100&_nc_cat=107&ccb=1-7&_nc_sid=5f2048&_nc_eui2=AeEKSsWB_Y9OkCzTBwQWfMfTDSM5Qc7oimwNIzlBzuiKbGs-axKefW64MBQvRVumLU7SUEtoKJ9cbM8SzDEHbTeV&_nc_ohc=S_JpdMjiUIYAb7_N3xo&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_Q7cD1QG4lCFcQa7DRkGOhpkHRh1dLIn2-a07hn0N5wYGThhLhw&oe=6657578C)

![](https://scontent.xx.fbcdn.net/v/t1.15752-9/437001493_1880604409071794_8216244788617306277_n.png?stp=dst-png_p160x160&_nc_cat=102&ccb=1-7&_nc_sid=5f2048&_nc_eui2=AeG9QpcDRmWfzgjJudfX0myXrCfnT8eUXnCsJ-dPx5RecOFfTMmoS3jErws4i6FAshAzzPXZdIDKqlopd_frqVcF&_nc_ohc=wXoiae1cv9QAb5yceRg&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_Q7cD1QH0lBPf2Q4_bwTuQAtXwtfnKYasVDlvTn9Hc_ZvEaYCTw&oe=66575581)



![](https://scontent.xx.fbcdn.net/v/t1.15752-9/437089157_801805358530828_2881197013834727166_n.png?stp=dst-png_s206x206&_nc_cat=101&ccb=1-7&_nc_sid=5f2048&_nc_eui2=AeHmeXlSNBVTBGbp4ITULcY_mf0lKIbrFwKZ_SUohusXAlQZhpBbLmkmznlFgV-_BpTVcLMejbO27pqW3S_QL27Z&_nc_ohc=dhoWazOCMj4Q7kNvgEC8zfO&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_Q7cD1QEXRIG5jeaaEUhS7W_M1eTh05EpeveftStgJHrIfqKSTg&oe=665760B9)


![](https://scontent.xx.fbcdn.net/v/t1.15752-9/438204615_354231607157376_1876487270217619274_n.png?stp=dst-png_p206x206&_nc_cat=106&ccb=1-7&_nc_sid=5f2048&_nc_eui2=AeEwwRr3he6qCUqm4OpD_uaqnkV_BBAShGieRX8EEBKEaB-ZKcP8FRuzcAIUpqGU9C5QKMibD6gP30iU9KIwgz5n&_nc_ohc=zIzXDhctNFIQ7kNvgHRIpei&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_Q7cD1QHro7WJ4BCsK95-rjJSl745B9z3gUQMYfXdop1v1vXGIg&oe=66573E0D)


# Algorithms
To represent the board, use a 2D array, each cell is a pokemon (represented by objects with unique ids) 
 
## Pathfinding algorithm 
- At its core of this game, is the pathfinding algorithm, in order to determine the path from pokemon A -> pokemon B, and to analyze its shape. 

- To simplify the problem: finding path from A to B with at most 2 turning points. Path should be return as a list of points (used for game path rendering) 

### My strategies 
- brute force: dfs or bfs, then tracing back the path to calculate the number of turning points, invalidate if it's > 2 
- to detect a turning point: we must know 3 consecutive points, if none of these 3 points is on the same axis (x == x or y == y) then a turn is made.
- A* (AStar): brute force is fine, but let optimize this a bit further, I chose AStar (A*) algorithm. 
	- I placed a heavy cost on turning point, g cost (cost to distance) has a much lower cost, I don't calculate f cost since it not needed.
	- I do a search first, then trace back the path, make sure it's <= 2 turning points
	- yes and that's it, it's more intentional when doing the pathfinding, compared to brute force, which reduce a lot of unecessary checks.    

![](https://private-user-images.githubusercontent.com/42113313/316292466-b733d684-81f2-44bf-af3c-c1bc18b66bbd.gif?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTQ0MTI3MTIsIm5iZiI6MTcxNDQxMjQxMiwicGF0aCI6Ii80MjExMzMxMy8zMTYyOTI0NjYtYjczM2Q2ODQtODFmMi00NGJmLWFmM2MtYzFiYzE4YjY2YmJkLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA0MjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNDI5VDE3NDAxMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPThiMjBkMDUxNTZhYWJlOTMxM2ViMGQyYzE5MTUxNTk0ODc3M2ViOGViZjc5NjAyNmE2YjY3NmU0ZGY1YzA3ZTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.yMS-Kk24MZfGxnGk9EV-ele8jVMhnovRNiikgPx_miQ)made with godot, [source](https://github.com/khuongduy354/astar-godot/)





## Suggest a valid pair algorithm 
- This algorithm should based on above pathfinding, currently I find no better solution than brute force. 
- Firstly, group pokemon with same images
- For each group, brute force run pathfinding on every pair (so n group => run n! times, O(n!) time complexity)
- Return first pair found ASAP. 

## Scramble board algorithm 
- First, I need to move remaining pieces into a list, then I scramble that list 
- Then I get all the placeable positions (in case the game has border or blocked cells) into the list, then I also scramble the list
- Then run the position list linearly and, keep popping the remaining pieces and place into position 



# Implementations (C++) 
C++, Terminal-based, windows console library

