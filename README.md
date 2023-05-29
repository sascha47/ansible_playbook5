# ansible_playbook5
configuration management
```
#configMgmt2
#20232205
# window and linux ansible commands. 
#I used a play book and for those commands that were not formmated, i used a basic adhoc ansible command
# which is found inside the word document with pictures


#ansible linux -m ansible.builtin.shell -a "echo $TERM" 

- name: server features # description of playbook
  hosts: windows1,linux1 # hosts that will be pinged based on hosts file
  tasks: # what playbook will do  
# os COMPLETE
    - name: find os
      ansible.builtin.debug:
        msg:  "Distribution:{{ansible_distribution}}"

  
#os_release       COMPLETE
    - name: os release
      ansible.builtin.debug:
        msg: "Release:{{ansible_distribution_version}}"

# fqdn complete
    - name: find fqdn
      debug:
        msg: "FQDN:{{ansible_fqdn}}"

    
# Shell linux COMPLETE
    - name: user shell (Linux)
     
      debug:
        msg: "Shell:{{ansible_user_shell}}"
      when: "'linux1' in inventory_hostname"

# # SOMETHING WRONG BELOW   
#     - name: user shell (Windows)
#       ansible.windows.win_shell:
#         _raw_params: $PSVersionTable.PSVersion
#       register: power
#       when: "'windows1' in inventory_hostname"

#     - name: print
#       debug:
#         var: power.stdout
#        ################### SOMETHING WRONG ABOVE

  #   #ls -l on root parition for linux  NOT FORMATTED

  #   - name: ls -l root partition
  #     ansible.builtin.command: ls -l /
  #     register: myoutput
  #     when: "'linux1' in inventory_hostname"

  #   - name: print
  #     debug:
  #       msg: "{{ myoutput.stdout }}"
  #     when: "'linux1' in inventory_hostname"


  # # Print the diskusage for LINUX  NOT FORMATTED

    - name: disk usage
      ansible.builtin.command: df -h
      register: diskUsageLinux
      when: "'linux1' in inventory_hostname"

    - name: print
      debug:
        msg: "{{diskUsageLinux.stdout}}"
      when: "'linux1' in inventory_hostname"
        

    # # Print the diskusage for Windows NOT
    # - name: disk usage windows
    #   ansible.windows.win_shell: Get-PSDrive
    #   register: usage
    #   when: "'windows1' in inventory_hostname"

    # - name: output disk usage windows
    #   debug:
    #     msg: "{{usage.stdout}}"
    #   when: "'windows1' in inventory_hostname"

    # - name: Windows Task list
    #   win_command: tasklist
    #   register: tasklists
    #   when: "'windows1' in inventory_hostname"

    # - name: output tasklist
    #   debug:
    #     msg: "{{tasklists.stdout}}"
    #   when: "'windows1' in inventory_hostname"
        
#create TEST directory for /
    - name: TEST in /
      ansible.builtin.file:
        path: /TEST
        state: directory
      when: "'linux1' in inventory_hostname"

# create test directory in  and c:\
    - name: test in c:\
      ansible.windows.win_file:
        path: C:\TEST
        state: directory
      when: "'windows1' in inventory_hostname"

# test directory owner linux
    - name: owner of test (linux)
      ansible.builtin.shell:
        chdir: /
        cmd: ls -ld TEST | awk '{print $3}'
      register: apple
      when: "'linux1' in inventory_hostname"
    
    - name: "output"
      debug:
        msg: "{{apple.stdout}}"
      when: "'linux1' in inventory_hostname"

# test direcotry owner (windows)
    - name: owner of test (windows)
      ansible.windows.win_shell: GET-Acl C:\TEST | Select-Object Owner
      register: apple
      when: "'windows1' in inventory_hostname"

    - name: owner of test (windows) output
      debug:
        msg: "{{apple.stdout}}"
      when: "'windows1' in inventory_hostname"





```
