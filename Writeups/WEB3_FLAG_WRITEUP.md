FLAG - PClub{y0u_ar3_in_7he_sudoers_th1s_1nc1d3nt_will_n0t_be_rep0r7ed}

Did 'ls' after connecting through netcat and found nothing

So ran 'ls -al' in /chal , saw .swp, .swl etc files, so found out that challenge is related to vim. however all files are owned by root so running 'vim -r <filename>' did not repair them and gave error.

Kept looking around in different directories and wasted a lot of time.

finally ran 'sudo -l' to list the superuser.

got '/bin/vim' as superuser with no password.

ran '/bin/vim' but there were font issues or something(as per gpt), so doing :!sh did not work.

force exited from the vim.

ran 'sudo vim -c "!sh"' trying to gain the root access.

after that, tried 'cd ..' trying to go back a directory

did 'ls' and saw root and then decided to go to cd root

did 'ls' saw flag(lfggg)

'cat flag' gave me the flag.