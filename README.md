# HCL-Day-15

# Task 01:

# code:

```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
wait = WebDriverWait(driver, 10)

try:
    driver.get("https://assertqa.com/practice/webtables")
    time.sleep(2)

    headers = wait.until(
        EC.presence_of_all_elements_located((By.XPATH, "//table//thead//th"))
    )

    header_texts = [header.text.strip() for header in headers if header.text.strip()]

    print("\n--- TC01: Column Headings ---")
    for idx, heading in enumerate(header_texts, start=1):
        print(f"Column {idx}: {heading}")

    print(f"\n✓ TC01 Passed: All {len(header_texts)} column headings displayed.")

finally:
    driver.quit()
```
# output:
<img width="687" height="210" alt="image" src="https://github.com/user-attachments/assets/521336a2-bb48-4c39-90ad-90e86ed94dff" />

# TASK02:

 # code:
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()

try:
    driver.get("https://assertqa.com/practice/webtables")
    
    wait = WebDriverWait(driver, 10)
    first_row_cells = wait.until(EC.presence_of_all_elements_located((By.XPATH, "//table//tbody/tr[1]/td")))
    
    print("--- TC02: First Data Row ---")
    
    record_data = []
    for cell in first_row_cells:
        record_data.append(cell.text)
        
    print("First employee record: " + " | ".join(record_data))
    
    print("\nExpected Result: First employee record displayed successfully.")

finally:
    driver.quit()
```

# output:

<img width="943" height="163" alt="image" src="https://github.com/user-attachments/assets/4a6c5c01-d238-4e27-bb20-4e2d25bcc8c0" />


# TASK03:

 # code:
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()

try:
    driver.get("https://assertqa.com/practice/webtables")
    
    wait = WebDriverWait(driver, 10)
    last_row_cells = wait.until(EC.presence_of_all_elements_located((By.XPATH, "//table//tbody/tr[last()]/td")))
    
    print("--- TC03: Last Data Row ---")
    
    record_data = []
    for cell in last_row_cells:
        record_data.append(cell.text)
        
    print("Last employee record: " + " | ".join(record_data))
    
    print("\nExpected Result: Last employee record displayed successfully.")

finally:
    driver.quit()
```

# output:
<img width="906" height="138" alt="image" src="https://github.com/user-attachments/assets/aa6a7ee9-7103-4468-b67f-c4b6e8e07516" />



# TASK04:

 # code:
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException

driver = webdriver.Chrome()

try:
    driver.get("https://assertqa.com/practice/webtables")
    
    target_name = "Smith" 
    
    wait = WebDriverWait(driver, 10)
    
    try:
        matching_row_cells = wait.until(EC.presence_of_all_elements_located(
            (By.XPATH, f"//table//tbody/tr[td[contains(text(), '{target_name}')]]/td")
        ))
        
        print("--- TC04: Search for Employee ---")
        
        record_data = []
        for cell in matching_row_cells:
            record_data.append(cell.text)
            
        print(f"Matching record for '{target_name}': " + " | ".join(record_data))
        print("\nExpected Result: Matching record displayed successfully.")
        
    except TimeoutException:
        print("--- TC04: Search for Employee ---")
        print(f"Result: Could not find any employee with the name '{target_name}'.")
        print("Fix: Please check the website and update the 'target_name' variable with a name that exists.")

finally:
    driver.quit()
```

# output:
<img width="1000" height="128" alt="image" src="https://github.com/user-attachments/assets/15af2d4e-a8e7-44fb-ba35-d1b83da44efc" />



# TASK05:

 # code:
```
import time
import re
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
wait = WebDriverWait(driver, 10)

try:
    driver.get("https://assertqa.com/practice/webtables")
    time.sleep(2)

    email_cells = wait.until(
        EC.presence_of_all_elements_located((By.XPATH, "//table//tbody//td[contains(text(), '@')]"))
    )

    emails = [elem.text.strip() for elem in email_cells]

    print("\n--- TC05: Extracted Email Addresses ---")
    for idx, email in enumerate(emails, start=1):
        print(f"[{idx}] {email}")

    print(f"\n✓ TC05 Passed: All {len(emails)} emails printed successfully.")

finally:
    driver.quit()

```

# output:
<img width="685" height="205" alt="image" src="https://github.com/user-attachments/assets/0b51ebc8-7a22-42b7-a975-739056400941" />

# Task 06:

# code:
```
import time
import re
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
wait = WebDriverWait(driver, 10)

try:
    driver.get("https://assertqa.com/practice/webtables")
    time.sleep(2)

    rows = wait.until(
        EC.presence_of_all_elements_located((By.XPATH, "//table//tbody/tr"))
    )

    highest_due = -1.0
    highest_record = None

    for row in rows:
        cells = [td.text.strip() for td in row.find_elements(By.XPATH, "./td")]
        if not cells:
            continue

        for cell in cells:
            match = re.search(r"\$?\s*([0-9]+(?:\.[0-9]+)?)", cell)
            if match and ("$" in cell or any(char.isdigit() for char in cell)):
                try:
                    val = float(match.group(1))
                    if val > highest_due:
                        highest_due = val
                        highest_record = f"{cells[0]} {cells[1]} (Amount: {cell})"
                except ValueError:
                    pass

    print("\n--- TC06: Highest Due Employee ---")
    print(f"Employee Record: {highest_record}")
    print(f"\n✓ TC06 Passed: Highest due employee identified successfully.")

finally:
    driver.quit()
```

# output:
<img width="725" height="146" alt="image" src="https://github.com/user-attachments/assets/0ee18c86-46a5-49a9-a099-79bed1ca1cfe" />

# Task 07:

# code:

```

```

# output:

# Task 08:

# code:

```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

driver = webdriver.Chrome()
wait = WebDriverWait(driver, 10)

try:
    driver.get("https://assertqa.com/practice/webtables")
    time.sleep(2)

    data_rows = wait.until(
        EC.presence_of_all_elements_located((By.XPATH, "//table//tbody/tr"))
    )

    row_count = len(data_rows)

    print("\n--- TC08: Table Data Row Count ---")
    print(f"Total Data Rows (excluding headers): {row_count}")
    print(f"✓ TC08 Passed: Row count successfully verified as {row_count}.")

finally:
    driver.quit()
```

# output:


