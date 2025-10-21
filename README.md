# GoogleWorkspace_DriveExternalShares
Google Workspace_Using the Apps Script function in Sheets, create a report of all shared files in your organizations domain and flag any files shared outside of the organization. This is meant to be a "workaround" for the limited availability of info out of the Google Workspace Admin dashboard.

Sample Project #5

The goal was to create a script that would parse and summarize sys log files. This would provide a count of event levels/ groups.

Steps:

1. Create a new Sheet. Go to Extensions > Apps Scripts.
2. Enter the following in the Code.gs window:

        function listSharedFiles() {
          const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
          sheet.clear();
          sheet.appendRow(['File Name', 'Owner', 'URL', 'Shared With']);
   
          const files = DriveApp.getFiles();
          while (files.hasNext()) {
            const file = files.next();
            const perms = file.getEditors().map(u => u.getEmail()).concat(file.getViewers().map(u => u.getEmail()));
            const external = perms.filter(e => !e.endsWith('@yourdomain.com'));
            if (external.length > 0) {
              sheet.appendRow([file.getName(), file.getOwner().getEmail(), file.getUrl(), external.join(', ')]);
            }
          }
        }

4. Save the project
5. Run manually once to authorize. Allow the requirements.
6. Open the linked Sheet to review the populated files.
