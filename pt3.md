### Scalability and Performance Results

- 20 consecutive refreshes were made to access the portfolio site using a Chrome with cache disabled. 
- Each test recorded the full page load time and DOM content loaded time using the Network tab.
**Screenshot of example refresh page:**
![Stats](/pics/05_Sample_Load_Stat.png)  
- Traffic was routed through HAProxy to two Nginx reverse proxy servers.

#### 🔍 Summary of Results

- **Average Load Time:** 1.49 seconds  
- **Average DOM Content Loaded Time:** 606.45 milliseconds  
- **No failures or timeouts occurred during testing**  
- Performance remained stable across all refreshes with minor variation

#### 📊 Test Table

![Performance Table](/pics/06_Result.png)

- The notes in the table clearly mark refreshes as "Normal", "Fast", "Slight Delay", or "Spike" based on observed load times. 
- The final row confirms an overall success of the load balancing strategy.
