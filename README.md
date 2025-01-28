# ABAP-RPS_GAME
Rock, Paper, Scissor game 
*&---------------------------------------------------------------------*
*& Report ZGAME_MUSK
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT ZGAME_MUSK.

types: BEGIN OF lty_rps,
       num TYPE I,
       name TYPE char10,
  END OF lty_rps.

DATA: lv_msg TYPE char50,
      lv_num TYPE I,
      user_choice TYPE char10,
      comp_choice TYPE char10.

DATA: LV_RPS TYPE TABLE OF lty_rps,
      Ls_rps TYPE lty_rps.

SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE text-002.
PARAMETERS: p_user TYPE I.
SELECTION-SCREEN END OF BLOCK b1.

SELECTION-SCREEN BEGIN OF BLOCK b2 WITH FRAME TITLE text-003.
  SELECTION-SCREEN:COMMENT 2(79) text-001.
*  SKIP.
*  SELECTION-SCREEN:COMMENT 2(79) text-001.
SELECTION-SCREEN END OF BLOCK b2.

INITIALIZATION.

ls_rps-num = 1.
ls_rps-name = 'Rock'.
APPEND ls_rps TO lv_rps.
clear ls_rps.

ls_rps-num = 2.
ls_rps-name = 'Paper'.
APPEND ls_rps TO lv_rps.
clear ls_rps.

ls_rps-num = 3.
ls_rps-name = 'Scissor'.
APPEND ls_rps TO lv_rps.
clear ls_rps.

START-OF-SELECTION.

Read TABLE lv_rps INTO ls_rps WITH KEY num = p_user.
user_choice = ls_rps-name.
CLEAR ls_rps.

CALL METHOD ZCL_GAME_MUSK=>RESULT
  EXPORTING
    PUSER         = p_user
  IMPORTING
    P_MSG         = lv_msg
    RAN_NUMBER    = lv_num
  EXCEPTIONS
    WRONG_INPUT   = 1
    others        = 2
        .
        .
IF SY-SUBRC <> 0.
MESSAGE 'Please enter valid input' TYPE 'I'.
ELSE.
  Read TABLE lv_rps INTO ls_rps WITH KEY num = lv_num.
  comp_choice = ls_rps-name.
  CLEAR ls_rps.
  WRITE:/ 'you choose',user_choice,
          /'computer choose',comp_choice,
          /'Result is',lv_msg.
ENDIF.
