![image](https://user-images.githubusercontent.com/41034062/173039301-aa449afc-974e-4a0f-85ac-27af29849c7f.png)


[W 2022-06-10 09:45:57.013 JupyterHub samlauthenticator:402] Got exception when attempting to parse SAML metadata
[W 2022-06-10 09:45:57.014 JupyterHub samlauthenticator:346] Exception: Unicode strings with encoding declaration are not supported. Please use bytes input or XML fragments without declaration.
[E 2022-06-10 09:45:57.014 JupyterHub samlauthenticator:684] Error getting SAML Metadata
[W 2022-06-10 09:45:57.014 JupyterHub base:768] Failed login for unknown user

remove encoding from metadata.xml file 
<?xml version='1.0' encoding='UTF-8'?>

[I 2022-06-10 10:03:48.416 JupyterHub samlauthenticator:558] The SAML Assertion matched the configured values
[D 2022-06-10 10:03:48.417 JupyterHub samlauthenticator:690] Authenticated user using SAML
[D 2022-06-10 10:03:48.417 JupyterHub samlauthenticator:695] Optionally create and return user: p007004
useradd: Permission denied.
useradd: cannot lock /etc/passwd; try again later.
[E 2022-06-10 10:03:48.536 JupyterHub samlauthenticator:635] Failed to add user by calling add user
[W 2022-06-10 10:03:48.537 JupyterHub base:768] Failed login for unknown user
[E 2022-06-10 10:03:48.538 JupyterHub web:1789] Uncaught exception POST /hub/login (::ffff:10.164.0.26)
    HTTPServerRequest(protocol='https', host='academy-jupyter.sapexperienceacademy.com', method='POST', uri='/hub/login', version='HTTP/1.1', remote_ip='::ffff:10.164.0.26')
    Traceback (most recent call last):
      File "/usr/local/lib/python3.8/dist-packages/tornado/web.py", line 1704, in _execute
        result = await result
      File "/usr/local/lib/python3.8/dist-packages/jupyterhub/handlers/login.py", line 160, in post
        login_error='Invalid username or password', username=data['username']


Final SAML Settings : 


  extraConfig:
     templates: |
       c.JupyterHub.template_paths = ['/srv/jupyterhub/custom']
     myConfig.py: |
       # c.JupyterHub.authenticator_class = 'nativeauthenticator.NativeAuthenticator'
       #c.Authenticator.admin_users = { 'useradmin1' }
       #c.Authenticator.admin_password = {'Welcome01'}
       c.JupyterHub.log_level = 'DEBUG'
       c.JupyterHub.authenticator_class = 'samlauthenticator.SAMLAuthenticator'
       c.SAMLAuthenticator.metadata_filepath = '/srv/jupyterhub/custom/metadataDevAcademy.xml'
       c.SAMLAuthenticator.time_format_string = '%Y-%m-%dT%H:%M:%S.%fZ'
       c.SAMLAuthenticator.audience = 'hub' # must exactly match the value in AWS
       c.SAMLAuthenticator.recipient = 'https://jupyter.sapexperienceacademy.com/hub/login'
       # c.SAMLAuthenticator.acs_endpoint_url = 'https://jupyter.sapexperienceacademy.com/hub'
       c.SAMLAuthenticator.organization_name = 'sapexperienceacademy'
       c.SAMLAuthenticator.organization_display_name = '''sapexperienceacademy'''
       c.SAMLAuthenticator.organization_url = 'https://sap.com'
       # c.SAMLAuthenticator.xpath_username_location = '//saml:Attribute[@Name="DottedName"]/saml:AttributeValue/text()'
       # c.SAMLAuthenticator.xpath_role_location = '//saml:Attribute[@Name="Roles"]/saml:AttributeValue/text()'
       #c.SAMLAuthenticator.allowed_roles = 'group1,group2'
       # c.SAMLAuthenticator.login_post_field = ''
       c.SAMLAuthenticator.nameid_format = 'urn:oasis:names:tc:SAML:2.0:nameid-format:emailAddress'
       c.SAMLAuthenticator.create_system_users = False
       c.SAMLAuthenticator.shutdown_on_logout = True
       c.SAMLAuthenticator.slo_forward_on_logout = False
