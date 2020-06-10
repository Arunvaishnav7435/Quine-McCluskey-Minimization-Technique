clear all;
clc;

%% Take binary input            COMMENT THIS SECTION FOR MINTERMS INPUT
%test inputs
% input examples:
%data = round(rand(1, 32));                % random data string  % highest tested number 8192 
%data = [1 1 0 0 1 1 0 0]
%data = [1 0 0 0 0 0 1]

%{
     data = input('data = ');
%}
     %OR
%{
     data = [         ];
%}

%%                            COMMENT THIS SECTION FOR DATA STRING INPUT
% input example: Min_terms = [0 1 2]
% Min_terms = [3] will give AND gate, [1 2 3] will give OR gate

Min_terms = input('Min_terms = ');
Min_terms = Min_terms + 1;
[hh, gg] = size(Min_terms);

Max = max(Min_terms);
nn = ceil(log2(Max));
mm = 2^nn;

if mm == 1
    mm = 2;                 % minimum value permited is two, atleast one variable
end 

for ii = 1:mm
   for jj = 1:gg
       if ii == Min_terms(jj)
           data(ii) = 1;
           break;
       else
           data(ii) = 0;
       end
   end
end

%%
[m, n] = size(data);            % n = size of data in bits

yy = log2(n);
j=1;

for i = 1:n
    if data(i) == 1
        min_terms(j) = i-1;             % get minterms
        j = j + 1;
    end
end

if j == 1                      % all zero input condition
    fprintf('all zero input');

elseif j == n + 1
    fprintf('all one input');

else

%% sort it

min_terms = unique(min_terms);
bin = de2bi(min_terms, yy);           % convert minterms to binary matrix
sol = bin;

next(1) = 1;
loop_count = 0;

%%  Tabulation method
while 1                      % loop until boolean algebra is solved

loop_count = loop_count + 1;
static = 0;              % check if bool is solved                
next(:) = [];             % to clean the variable in next run
next(1) = 1;
[m, p] = size(sol);   % m = number of rows, p = number of columns
q = m - 1;           % we only have to visit m - 1 column
for i = 1:q
   
    d = bin(i,:) == bin(:,:);         % comparing number of equal terms
    val = sum(d == 0, 2);               % counting number of equal terms
    p_val = size(val);                 % p_val = number of rows of val    
    
    for k = i:p_val               % p_val = number of rows of bin 
        check = sum(bin(i,:) == -1) == sum(bin(k,:) == -1);
        if val(k) == 1 & check == 1
                med = bin(k, :);               % temperory variable
                med(find(d(k, :) == 0)) = -1;  
                sol(end+1,:) = med;
                next(end+1) = k;             % store k row to be removed
                next(end+1) = i;             % store i row to be removed
                static = static  + 1;       % increment in static
        end
    end
end
next(1) = [];                  %first element is useless
sol(next(:), :) = [];          % remove rows from sol
sol = unique(sol, 'rows');      
bin = sol;
if static == 0
    break;                       % break when boolean is solved
end

end

%% printing result

count = 1;
[ro, co] = size(sol);
sol(:, 1:co) = sol(:, co:-1:1);
loop_count;
%fprintf(' number of rows = %d and number of columns = %d \n', ro, co);
for s = 1:ro
    flag = 0;
  for r = 1:co
    
      if r ~= 1 & flag ~= 0 & sol(s, r) ~= -1
            fprintf('.');                       % printing . sign
      end
      if sol(s, r) ~= -1
          fprintf('A%d', r - 1);                 % printing variable
          flag = flag + 1;
      end
      if sol(s, r) == 0
              fprintf('`');                      % printing prime
      end
      
  end
  
  if s ~= ro                      % if condition allows not print last unnecessary '+'
    fprintf(' + ');               % printing sum sign
    count = count + 1;
  end
  
end

end
fprintf('\n');