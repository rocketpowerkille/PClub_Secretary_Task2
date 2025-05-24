Flag - PClub{y0u_ar3_in_7he_sudoers_th1s_1nc1d3nt_will_n0t_be_rep0r7ed}

ran ls  -al in /chal , saw .swp, .swl etc files, so challenge is related to vim. however all files are owned by root so running vim -r 

<filename> did not repair them.,

looked around in different directories and wasted a lot of time.,

finally ran sudo -l to list the su.,

got /bin/vim as su.,

ran /bin/vim but there were xterm(fort+bad tty issues), so doing :!sh did not work.(!sh it spawns a shell in vim),

ran sudo vim -c "!sh" which gave me root.,

after that, cd .. then cd root which had only 1 file,

cat flag gave me the flag ez.