# 👤 Add and Manage Users with Linux Commands

**Course:** Google Cybersecurity Certificate
**Environment:** Linux Bash shell
**Skills practiced:** User authentication management, group management, file ownership, least privilege

---

## 📌 Scenario Overview

Authentication is the process of a user proving they are who they say they are. As a security analyst, part of my job is making sure that not everyone gets access to the system, that new employees are added, and that users who change groups or leave the organization are updated or removed.

In this lab, a new employee with the username **researcher9** joined the Research department. I managed their access through their whole time at the organization:

1. Added them to the system and to their primary group
2. Made them the owner of a project file
3. Added them to a secondary group after a role change
4. Deleted them from the system when they left

> ⚠️ All commands used `sudo` because adding and removing users and groups requires root privileges.

---

## 🛠️ Commands Used

| Command | Purpose |
|---|---|
| `useradd` | Add a new user |
| `usermod` | Modify a user (change groups) |
| `chown` | Change file ownership |
| `userdel` | Delete a user |
| `groupdel` | Delete a group |

---

## ➕ Task 1: Add a New User

A new employee joined the Research department, and I added them to the system with the username `researcher9` and the primary group `research_team`.

**Command I used (one step):**

```bash
sudo useradd researcher9 -g research_team
```

This creates the user and sets their primary group at the same time.

**Alternative (two steps):** The same result can be done by creating the user first, then changing the primary group with `usermod` and the `-g` option:

```bash
sudo useradd researcher9
sudo usermod -g research_team researcher9
```

📸 **Screenshot:**

<img width="1096" height="210" alt="task1-add-user" src="https://github.com/user-attachments/assets/46750b19-2e3b-462d-ae64-285501a3e86f" />
<img width="1047" height="126" alt="task1-add-user_continuation" src="https://github.com/user-attachments/assets/32e5ad5e-a60c-4eb1-8a65-eba36452d8d2" />

---

## 📁 Task 2: Assign File Ownership

`researcher9` took responsibility for project R, so I made them the owner of `project_r.txt`, located in `/home/researcher2/projects` and originally owned by `researcher2`.

```bash
sudo chown researcher9 /home/researcher2/projects/project_r.txt
```

📸 **Screenshot:**

<img width="1107" height="325" alt="task2-chown" src="https://github.com/user-attachments/assets/cf248251-dfd9-4a9c-897e-eda9f780c608" />

---

## 👥 Task 3: Add the User to a Secondary Group

A couple of months later, the employee's role changed and they began working in both Research and Sales. I added `researcher9` to the `sales_team` group as a **secondary group**, while their primary group stayed `research_team`.

```bash
sudo usermod -a -G sales_team researcher9
```

- `-a` means **append**, so the user is added without being removed from other groups.
- `-G` (uppercase) specifies the **supplementary group**.

> 💡 Linux options are case-sensitive: lowercase `-a` and uppercase `-G`.

📸 **Screenshot:**

<img width="1091" height="325" alt="task3-secondary-group" src="https://github.com/user-attachments/assets/90a7f7e5-8a72-4b6c-a03c-4ca7c0520e14" />

---

## 🗑️ Task 4: Delete a User

A year later, `researcher9` left the company, so I removed them from the system.

```bash
sudo userdel researcher9
```

This returned the message:

```
userdel: group researcher9 not removed because it is not the primary group of user researcher9
```

This message is expected. When a user is created in Linux, a group with the same name is also created, and the user is its only member. Here the user's primary group was `research_team`, so `userdel` left the `researcher9` group behind. Good practice is to clean up these empty leftover groups:

```bash
sudo groupdel researcher9
```

📸 **Screenshot:**

<img width="1100" height="357" alt="task4-delete-user" src="https://github.com/user-attachments/assets/00871947-c9ea-49c9-8f7a-045bf34abfff" />
<img width="1086" height="362" alt="task4-delete-user_continuation" src="https://github.com/user-attachments/assets/2aecfb38-fde2-49f2-b98e-2332e27d0911" />

---

## ✅ Summary

In this lab, I managed the full lifecycle of a user account in Linux:

- **Added** a new user and assigned their primary group with `useradd`
- **Assigned file ownership** to the new user with `chown`
- **Added** the user to a secondary group with `usermod -a -G`
- **Deleted** the user with `userdel` and cleaned up the leftover group with `groupdel`

### 🔐 Key Takeaways

- Managing users and groups is a core part of controlling **who has access** to a system.
- Access should be updated whenever someone joins, changes roles, or leaves which supports the **principle of least privilege**.
- Leftover accounts and empty groups can become security risks, so cleanup matters.
- Always use `-a` with `-G`, otherwise the user's existing secondary groups can be overwritten.
