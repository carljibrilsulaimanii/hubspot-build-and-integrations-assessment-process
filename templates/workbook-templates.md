<!--
  workbook-templates.md -- column headers for every tab of the assessment workbook
  Author:  Jibril Sulaiman
  Date:    2026-10-19
  What:    One CSV header per tab, in the order the steps fill them. Paste each into a
           sheet tab of the same name.
  Why:     The workbook is the assessment's single output; consistent columns let the
           risk register and the scorecard reference rows by id.
-->

# Workbook templates

Create one tab per block below and paste the header line as row 1.

## Access (Step 1)

```csv
system,role_granted,granted_by,granted_on,notes
```

## Answers (Step 2)

```csv
person,group,question_number,verbatim_answer,theme,step_that_tests_it,hypothesis_id
```

## CTO questions (Step 2)

```csv
number,question,one_line_answer,proved_in_step,evidence_link
```

## Users and seats (Step 3a, 3b)

```csv
user,email,seat_type,permission_set,super_admin,team,employer,last_login,start_date,departure_plan,status,owner
```

```csv
seat_type,bought,assigned,cost_per_seat,renewal_date,approver,notes
```

## Domains (Step 3c)

```csv
host,platform,role,dns_host,ssl,redirects,primary_domain,tracking_code,tag_manager,cookie_banner,who_can_add,status,owner
```

## Email, SMS, phone (Step 3d to 3f)

```csv
item,type,setting_or_value,where_recorded,known_issue,status,owner
```

```csv
line,number_or_name,line_type,registration,inbox,agent,hubspot_channel,opt_out_handling,quiet_hours,consent_capture,rate_per_segment,workflows_using_it,status,owner
```

## Payments (Step 3g)

```csv
payment_type,processor,product,live_or_test,where_it_lands_in_hubspot,exception_handler,known_issue,status,owner
```

```csv
payment_link,product,price,after_payment_redirect,live_or_test,order_bump,pages_using_it,status,owner
```

## Objects (Step 3h)

```csv
object,type_id,standard_or_custom,record_count,count_read_at,created,creator,writers,readers,unique_key,associations,property_groups,record_customization,naming_convention,status,owner
```

## Pipelines (Step 3i)

```csv
pipeline,stage,order,win_probability,set_by_workflow,outcome_fields,owner_assignment,deal_count,amount,count_read_at,notes
```

## Workflows (Step 3j)

```csv
workflow_id,name,family,object,on_off,trigger,re_enrollment,creator,last_editor,last_edit,custom_code_actions,secrets_used,webhook_actions,third_party_actions,enrolled,errors_30d,revisions,flag,status,owner
```

## Forms and pages (Step 3k)

```csv
item,type,url_or_id,fields_or_modules,hidden_utm_fields,consent_text,triggers_workflows,head_html_blocks,payment_links,pixels,known_issue,status,owner
```

## Lists, reports, dashboards (Step 3l)

```csv
item,type,id,size_or_widgets,used_by,used_as_trigger_or_audience,quoted_where,last_viewed,status,owner
```

## Tech stack (Step 3m)

```csv
tool,role,account_owner,users,monthly_cost,hubspot_connection,status,notes
```

## Integrations, keys, secrets (Step 3n, 3o)

```csv
integration,direction,mechanism,owner,key_or_secret,test_result,failure_signal,who_gets_paged,status
```

```csv
service_key,prefix,key_id,scopes,consumers_from_view_logs,shared,deprecation_date,risk,owner
```

```csv
secret,created,purpose,consumer,confidence_observed_or_inferred,notes
```

## Outside automation (Step 3p)

```csv
automation,runs_where,trigger,writes_into_hubspot,owner,last_successful_run,status
```

## Property register (Step 4c)

```csv
object,internal_name,label,type,group,created_by,created_on,fill_rate,used_in_workflows,used_in_lists,used_in_reports,used_in_forms,used_in_cards,writer,lag,verdict,referenced_by_assets,proposed_action
```

## Trusted numbers (Step 5)

```csv
metric,definition,tool,exact_query,exclusions,current_value,read_at,owner,second_version_exists,reason_for_gap
```

## Scenarios (Step 7)

```csv
scenario,trigger,expected_trail,where_verified,record_links,verdict,failure_seen,who_would_notice
```

## Measurement chain (Step 8)

```csv
hop,what_is_captured,what_is_lost,ratio_observed,ratio_expected,owner
```

## Risk register (Steps 9, 11)

```csv
id,risk,evidence,likelihood,impact,blast_radius,owner,fix_build_doc_id,status
```

## Scorecard (Step 11)

```csv
area,score_1_to_5,evidence,one_change_to_move_one_point
```

## Known items (Step 11)

```csv
id,area,item,date_found,evidence,fix_build_doc_id,status
```
