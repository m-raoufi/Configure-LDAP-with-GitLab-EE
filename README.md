# Configure-LDAP-with-GitLab-EE
Sync Active Directory users with GitLab and you can login with AD users in GitLab

The initial configuration of LDAP in GitLab requires changes to the gitlab.rb configuration file (/etc/gitlab/gitlab.rb). Below is an example of a complete configuration using an Active Directory.

    Example gitlab.rb LDAP
    
            gitlab_rails['ldap_enabled'] = true
            gitlab_rails['ldap_servers'] = YAML.load <<-'EOS'
      ###! **remember to close this block with 'EOS' below**

          main: # 'main' is the GitLab 'provider ID' of this LDAP server
            label: 'LDAP'
            host: 'dc.example.com'
            port: 389 # or 636
            uid: 'sAMAccountName'
            method: 'plain' # "tls" or "ssl" or "plain"
            bind_dn: '_the_full_dn_of_the_user_you_will_bind_with'
            password: '_the_password_of_the_bind_user'
            active_directory: true
            allow_username_or_email_login: false
            block_auto_created_users: false
            base: 'OU=example INT,DC=example,DC=com',

    Note: Remember to run gitlab-ctl reconfigure after modifying gitlab.rb
