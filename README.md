#透過使用google fusion tables來做GIS
GIS是geographic information system的簡稱，即「地理資訊系統」。QGIS之類的軟體，已經算是容易上手了。然而在不要求很複雜的呈現的前提之下，最簡單的作法，應該是使用google fusion table。

##如何啟動Google fusion table
1. 開啟瀏覽器，登入gmail，點選右上方的「九格」圖示，並且進入「google 雲端硬碟」  
2. 在雲端硬碟裡，點選「新增」，然後點選「更多」  
3. 如果沒有看到「Google Fusion Tables」 的話，就點選「連結更多應用程式」。並且在「搜尋框裡」輸入「Google Fusion Tables」加以連結。爾後就可以在第2步的新增，直接新增「Google Fusion Tables」
4. 準備Google Fusion Tables需要的CSV檔。  
為了簡單，我們直接下載一個事前準備好的CSV檔。CSV檔是comma separated value的縮寫，即「英文逗號」分隔的檔案。用純文字編輯器即可編輯。[下載的網址](https://github.com/humorless/taiwan-village-KML/) 在此處下載 panhan3-GIS.csv 這個檔案。可以點右邊的「Download ZIP」來下載。

##開始制作GIS
1. 點下之後，Google Fusion Tables啟動之後。
  它會先問第一個問題，「你要從哪邊import new table」
  由於google fusion tables的操作介面，並不是很容易輸入資料，建議選「From this computer」，然後上傳已經準備好的CSV檔。
	* 選「From this computer」
	* 選擇檔案 --> 從自己電腦找出 panhan3-GIS.csv
	* Separator character --> 選 Comma
	* Character encoding  --> 選 UTF-8
	* 選「Next>>」
2. Column names are in row [1]
	* 選「Next>>」  

	> 這個的意思是說，第一列，我們指定它為欄位名稱。

3. 在Table name的地方填你喜歡的table name。例如panhan3-GIS
	* 選「Finish」  

	> 如果CSV的原始資料不想讓網路上的人可以取得，就要把Allow export取消。
	> 
4. 選Map of location，就可以看到GIS圖。  
	
	> 之所以可以看到GIS圖，是因為在上傳的CSV檔中，location欄的KML資訊會被畫成一塊又一塊的村里邊界。  
	> 然而，此時的圖只有一種顏色。所以接下來要讓圖的每一區塊可以根據里的得票率來變色。

5. 在Map of location的左下方，點選Change feature styles  
	* 點選Polygons下方的Fill colors
	* 點選Polygon background colors下方的Gradient
	* 點選Show a gradient  

	>這個動作是讓多邊形(即polygon)的背景色會漸層著色(gradient)

6. Column選擇「得票率」。
	* 點選「use this range」
	* 點選「Save」  

	>這個動作是讓漸層的著色，根據「得票率」來著色。

##客制化有KML資訊的CSV檔。
1. 台灣歷年選舉的得票資料，可以到中選會的網站抓。 
2. 在 https://github.com/humorless/taiwan-village-KML/ 可以下載的「全台村里界的kml」資料。其中taiwan_village.kml.csv 和 taiwan_village_db 裡頭是同樣的資料。

	>由於taiwan_village.kml.csv相當大，不容易做篩選。而且可能會讓excel當掉。建議使用taiwan_village_db。

3. 如果為了效率，要使用taiwan_village_db的話，要先在windows電腦上安裝SqliteBrowser。用SqliteBrowser可以直接開啟taiwan_village_db


#Sqlite簡介
1. 用SqliteBrowser開啟taiwan_village_db  
2. 選「Execute SQL」  
3. 在『SQL 1下第一個視窗』內，鍵入指令 `select * from taiwan_village;`

	>注意，一定要加英文的分號

4. 按F5, 執行指令。  
   如果成功，『SQL 1下第二個視窗』會出現篩選出的資料。  『SQL 1下第三個視窗』會出現 7800 Rows returned from: select * from taiwan_village; (took 460ms)  
5. 此時，可以選『SQL 1下第三個視窗』右方的按鈕，選「export to CSV」。  

	> 如此，就可以把剛才的篩選結果，匯出成為有KML資訊的CSV檔。

##其它可以考慮嘗試的SQL commands：  
1. where 可以用來設定「條件」  
	` select * from taiwan_village where county = "臺北市文山區";`  
2. % 類似regex 的 .*  
	`select * from taiwan_village where county like "臺北市%";`  
3. *表示所有的欄位。也可以一一列舉。  
	`select county, village, kml from taiwan_village where county = "臺北市文山區" or county = "臺北市大安區";`  
4. || 表示合併  
    `select county||village, kml from taiwan_village where county = "臺北市文山區" or county = "臺北市大安區";`